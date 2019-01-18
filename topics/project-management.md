# Project Management Notes

## Project Planning

* Important considerations:
  * Scoping
    * Are you trying to build more than you can in the time allotted?
      * If so, what compromises can you make?
    * Are you building too much for initial release?
      * Would it be better to build something smaller and release it so you can learn?
  * Release gating
    * How will you keep the work private until it's ready for production use?
      * e.g. Feature flags, Git branch
    * Will you need a kill switch in case something goes wrong after go-live?
    * Do you need to ramp the release instead of releasing to everyone?
      * e.g. Via an A/B test or incremental ramp-up
  * Monitoring
    * How will you know the system you're building is working?
    * How will you know if it breaks?
    * Could be *active*
      * e.g. Automated alerts to on-call engineers or to teams
    * Could be *passive*
      * e.g. Dashboards or logs you can look at
  * Scaling
    * What will the system need to succeed under production load?
      * It has to work on more than an engineer's machine and in automated tests
  * Backwards Compatibility
    * If updating an existing system that other system(s) depend on, how do you keep from breaking
      those other system(s)?
      * e.g. APIs and mobile app clients

## Writing Stories/Tickets

* Parts:
  * Background
    * Why are we doing this work?
    * Any important context/knowledge someone working on this should have?
  * Requirements
    * What things should be true once the work is complete?
    * e.g. Data that should be persisted, functionalities a UI should support, etc.
    * If building something visual, should enumerate the important aspects of the design
      * Don't assume engineer will notice a detail just because the design contains it
    * Should describe *what* to build, but not *how* to build it
    * Can include technical requirements
      * e.g. Log messages must be recorded, analytics events must be sent
    * Can include technical constraints
      * e.g. Don't use some library we already have because we're deprecating its use
  * Technical Notes
    * What technical things would be helpful for the engineer(s) working on this to know about?
    * e.g. Existing code they can utilize or take inspiration from
  * Acceptance
    * How can someone verify that the work done met the requirements?
    * Indicate if a Product Manager can perform acceptance, and if they'll need an engineer's help
      * As opposed to a technical story that an engineer has to accept
    * Describe:
      * What environment to test in
      * What kind of user to use
      * Any setup steps
        * e.g. Enable a feature flag, get a user account into a certain state, etc.
      * If UI work:
        * What interactions to perform to check the requirements
        * Any accessibility, internationalization, etc. concerns to check on
      * How to verify all requirements
      * Any security precautions or authorization logic and how to verify them
* Important Considerations:
  * Analytics events
    * Names
    * Data to record
  * Accessibility
  * Internationalization
  * Error behaviors
    * e.g. For forms, background workers, etc.
  * UI states
    * Do the requirements cover handling invalid user input, success behavior, etc.
  * Complexity
    * Is the work too complex to have in one story/ticket?
    * Can we complete the work in the time allotted?
    * If time is short, are there any requirements that are "nice-to-have" and could be broken out
      to do later if time permits?
  * Security
    * e.g. Safely handling user-generated input
  * Authorization
    * Make sure only those who are supposed to use it can use it
    * And only in the ways you intend

## Project QA

* Before releasing/launching work, check:
  * Feature flags
    * If there are any, double-check they work as intended
  * Double check important concerns like accessibility, internationalization, etc.
  * If a web UI, double check it in all browsers you support
  * Any external systems that will depend on the one you're releasing
    * e.g. API consumers
* If possible, double check the system works as expected in production
  * If a new UI feature, see if you can release it to internal company users for testing

## Project Launch

* Notify all relevant stakeholders
  * Could be inside or outside of company depending on project
  * Make sure to give them sufficient time before go-live
  * Don't forget about on-call engineers
* Set up any A/B tests
* Activate any feature flags

## Post-Launch

* Schedule when to follow up on impact of project since release
* If release is behind an A/B test or partial ramp, determine when to decide whether to ramp further
  * What criteria would need to be met in order for the project to ramp further?
* If release behind a feature flag, determine when and what criteria must be met for flag to be
  removed/cleaned up
* Create follow-up stories/tickets to address any technical debt or refactoring left over from the
  project
