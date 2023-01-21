# TDD
Technical Design Documents

See [Writing a TDD](https://medium.com/machine-words/writing-technical-design-docs-71f446e42f2e) if you are unfamiliar with them.

Remember, TDD's exist to avoid restarting a project multiple times because there is a roadblock that could have been avoided with the proper planning.

## Do's and Dont's

- Do's:
  - Before sending your TDD to others for review, take a pass at it from the reviewer's perspective. What questions and/or doubts might you have about this design? Address them right away. Also, think to yourself... if another engineer has to pick this project up, or if you go on vacation, can anyone read this and implement the solution as designed?
  - Divide your work into bite sized phases. Each phase will represent one PR, this helps track which changes broke and allows for smaller (and quicker!) code reviews.
  - Work with someone else if you'd like, you don't have to do this alone!

- Dont's:
  - Do not be complacent. TDD's are boring but crucial for an optimal developement process and an extremely important skill to possess in the industry.
  - Don’t start without a structure. What is meant by this is to start by taking short notes, and once all those are good you can start expanding on it in greater detail.
  - Don’t forget your punctuation and grammar. I know, I know, boring. This is crucial to avoid any confusion though, so before any TDD is comitted, run it through a spellchecker

## How to write a TDD for Firefly

- [Fork](https://github.com/FireflyOS/TDD/fork) this repository.
- Create a new branch and write a TDD.
- Submit a PR.

## Folder structure

`tdd/` - All TDD's related to firefly will be located in this folder

`TDD_Template.md` - A template you will copy, paste and alter. All TDD's will be based on this template.
