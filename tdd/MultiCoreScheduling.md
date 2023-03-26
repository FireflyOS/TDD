# Low Level Multi Core Scheduler Abstraction

Authors: @V01D-NULL

Date: March 12 2023

### Introduction

Our SMP aware low-level scheduler abstraction will allow for faster and concurrent execution of both kernel and user tasks while balancing the system's workload across all available processing units in an equal fashion. This scheduler will NOT focus on the algorithms, but rather the concurrency challenges that come with an SMP compliant system. This smp scheduler merely abstracts most low-level details of communicating and balancing work, it is a foundation which can be utilized by implementing scheduling algorithms to decide what to schedule when. This component merely handles low level communication.

### Terminology

MP = Multi Processing

SMP = Symmetric Multi Processing

#### GOALS

- Scheduling algorithms can be changed during compile time with little to no changes to the code.
- Spend as little time waiting for work as possible.
- Use as few spinlocks as possible

#### NON GOALS

- Any type of scheduling algorithms. We will purely focus on the smp aspect of it.
- Adding support for a single core scheduler.
- Anything related to userspace or ring level transitions.
- Implement preemption/non-preemption. This is up to the scheduling algorithm implementation.

### Requirements

It is assumed the that computer utilizing this scheduler has 2 or more healthy and available processing units, each of which are required to be in long mode with all initialization complete and awaiting an IPI in an Idle loop to beging scheduling tasks.

### Architecture

A modular design where each core has the authority to give up tasks or pull in new tasks will allow for a stable, coherent system where a dying or dead core is not at risk of bottlenecking the entire system as could be the case with a master and slave architecture with one core to manage others.

Additionally, each task has the right to bind any number of tasks to itself if necessary, meaning such a task is not available for a load balancing procedure. This helps improve performance as the kernel do not need to waste cycles on IPI's as well as flush and repopulate the cpu cache for the particular process in question.

This can be helpful for certain kernel tasks, but user level tasks would also be able to utitlize this using some kind of syscall to update their processor affinity.

When certain cores are under a heavy workload, they must scan the availablity of other cores in no particular order. A suitable core would be chosen based on the lowest load percentage when compared to all cores in the system excluding the core currently scanning for a free worker (core).

In order to minimize the performance overhead load-balancing a task to another core incurs, the kernel attempts to enforce a soft affinity policy unless it is impossible to do so, in which a task must switch cores. Workloads will be balanced across cores using an altered version of the pull migration strategy in which a core may scan for work if it's workload is below a certain threshold as oppose to only stealing work when the core has no tasks in it's run queue.

## Phase 1:

For this first phase, the main goal is to get each core setup with it's own private data (process queue) using some sort of cpu-local/per-cpu storage.

### Design

Upon smp initialization, each core receives a predetermined memory address for it's local and private data (variables), the base address of this local data will be stored by leveraging the architecture's gs-base for easy and quick access. We will not store this information in the `AssemblyCpuData` structure to avoid loading large amounts of data from gs-base since per-cpu variables are likely to be accessed frequently. These memory addresses will be fixed in size and determined at link time, similar to linux's PER_CPU() model. Future revisions might allow for the per-cpu memory size to grow and shrink.

Pseudo code implementation:

```c++
// Low level smp handling.
// This is what the TDD is about
class Base<typename Store> {
private:
    PER_CPU_VARIABLE(Store) process_queue; // This would contains tasks a core would process
};

// ...

// Scheduler implementations using the low-level abstraction
class SchedulerImpl : Base<vector> {

};

class SchedulerImpl : Base<linked_list> {

};

class SchedulerImpl : Base<priority_queue> {

};

// ...

// Import desired scheduler
#ifdef SCHEDULER_1
#include "scheduler1.hpp"
#else if SCHEDULER_2
#include "scheduler2.hpp"
#else if SCHEDULER_3
#include "scheduler3.hpp"
#else
#error "No scheduler implementation"
#endif

void init_smp(AssemblyCpuData* cpu, uint64_t per_cpu_memory) {
  // Store per-cpu base address
  write_gs_base(available_gs_base_offset, per_cpu_memory); // Scheduler can be accessed from the per-cpu variable now. I'm not sure if we need this anymore though, because we could simply read_gs_base and get the cpu structure. My only concern with doing it like this is the potential overhead of loading a large structure from gs-base (i.e. memory) and only using a portion of it very frequently.

  cpu->scheduler = allocate_from_this_per_cpu_memory<SchedulerImpl>(); // Create and store the scheduler in the per-cpu area to create a true, private variable. Allocating it via 'new' might be unsafe as the memory area 'new' manages is not protected where as per-cpu variables are completely private and hidden from any allocators. That is to say allocators have no idea this memory even exists.
  write_gs_base(available_gs_base_offset_2, cpu);
}

```

### Data Model

Scheduler implementations are to inhert from the base class that manages all smp related tasks, including load balancing. There may only be one base class, but multiple scheduling algorithms to inherit this base class as long as there is only one active at any given time.

The scheduling algorithm can be determined at compile time and has a default to fallback to when no implementation was specified.

### Breaking changes

N/A

## Phase 2:

TODO: ....begin talking to other cores to initiate load balancing

> Repeat design, data model and breaking changes for each phase

<hr/>

### Security

It is possible that the scheduler can run into concurrency bugs leading to processes or the kernel revealing sensitive data, or the entire system runs the risk of becoming unstable once again allowing the potential for the kernel to leak sensitive data.
