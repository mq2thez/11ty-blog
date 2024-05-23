---
title: Teaching Project Leadership
description: Helping junior engineers learn how to lead projects
date: 2024-05-17
layout: layouts/post.njk
tags:
  - career-development
---

I won't claim that my style of teaching project leadership is unique or novel, but I've done things this way for a number of years and with a lot of teams, now. I call it "primary/secondary". In a primary/secondary project, a specific feature is assigned to the "primary", and they are partnered with a "secondary". The "primary" in this pairing is the person who has something to learn, and the "secondary" is the person whose job is to understand all of the details and enable that person to learn as effectively as possible. During the entire arc of the feature development, the primary and secondary work very closely on all aspects of planning, organizing, and documenting the work. Sometimes, but not always, the secondary engineer will contribute code for the project. Other times, they will be focused entirely on enabling the organization and planning work. There may be other engineers who work on the actual implementation once the planning is complete, but we're focused here on how the project is planning and executed, rather than the specific implementation.

## Role of the Primary

The goal for the primary is to _learn_. We want the primary to get experience in a psychologically safe way, so that they feel comfortable asking questions and trying things out. They need to ask questions, push the secondary for more knowledge, and above all else, seek to gain an understanding of _why_ they should be doing things in one way or another. They should be viewing everything from the lens of needing to be able to do this work on their own in the future. For junior engineers on their first big projects, this means getting a deeper understanding of how to break features down, plan for delivery dates, and define success. For more experienced engineers, this means feeling confident that they can plan for multiple streams of work to be handled by other engineers.

When working on a project like this, a primary can be said to have succeeded if they come out of it with a good understanding of how to plan and execute on a similar sized future feature without any help. In this light, it doesn't specifically matter whether there were problems with launching the feature in question. What matters is whether the primary learned from the problems they encountered and feels confident that they would be able to anticipate or avoid such problems in the future.

## Role of the Secondary

The goal for the secondary is to _enable_ the primary. The secondary's focus should be on providing a paved path for the primary. When paired with junior engineers acting as primary on their first project, the secondary will often be explaining a lot of their own method of thinking. A common phrase I might use in this is, "When I start to break down a feature for planning it, this is what I think about". An important aspect here is that a secondary should provide _examples_ for the primary, but try very hard not to then actually break the current feature down for them. In order to facilitate learning, provide related examples and suggestions, but ensure that the primary is the one who actually takes each step for the current project. It won't always be possible to do perfectly, either due to the temperment of the primary/secondary or else because some folks do a lot better when shown rather than guided.

When working on a project, the secondary should (whenever possible) have a good idea about what potential problems the primary will run in to, and be prepared to guide the primary towards these gently. Leading questions are great for this, and can involve longer discussions. For example, if the primary isn't sure about how to start breaking down a feature, it might be good to ask questions like, "Would it make more sense to implement the frontend or backend first?" and then have a discussion about the various trade-offs. As the discussion wraps up, bring things back to the original question, "Would it make more sense to implement the frontend or backend first?" and ensure that the primary is able to apply the theoretical discussion to the practical example in front of them.

The secondary should also be prepared to remove blockers for the primary. For some projects, there may be specific parts of a feature which would be significantly more complicated for the primary to understand/implement. Because the goal for this is to teach people how to run a project rather than specifically improving their coding skills, the secondary would handle the blocker by saying, "I'll handle this particular aspect of the feature, it's enough that you know it exists as an issue", and then move on. Examples of this might be something like setting up a particularly complicated series of backend data fetches in order to ensure optimal data fetching patterns, setting up Redux stores or creating the initial code for a React project, etc. These are very valuable skills for a junior developer to learn, but stopping to dig deep on understanding all of the aspects of them would be a distraction from their main goal of learning how to plan and execute on a project.

## Deciding Whether To Use Primary/Secondary

Primary/Secondary works best for medium or small projects. It shines in situations where a single person (the primary) can be reasonably expected to plan and lead the entire project. It is _not_ a good choice for projects which might have features which are themselves medium-sized projects. That is, if a project is so large that its features require their own independent research, planning, and delivery dates, then those smaller features would be good candidates, but the overall project would not. The reason for this is that at that scale, it's not feasible or desirable to have two people trying to manage all of the planning and organizing work -- it is the work for an entire team.

Primary/Secondary can work for larger projects if your team runs the risk of being heavily siloed. When I've worked on platform or infrastructure teams, projects were often long-running efforts by one or two people. We used Primary/Secondary to keep everyone supported and ensure that no one was going off the rails or getting stuck. It also gave us a great strategy for developing more junior engineers, for whom platform/infrastructure work can be _extremely_ difficult to ramp up in. In these situations, we might have a junior engineer who is the primary on a specific feature, and a more experienced engineer who is the secondary on that project while also working as the primary on a different project (with their own secondary there). This creates a much better sense of cohesion and develops team-wide best practices than would exist if everyone were off doing their own thing.

Primary/Secondary is probably not a great choice on teams with high-pressure deadlines. People can definitely grow in those sorts of situations, but being under significant pressure does not create the kind of psychologically safe spaces that a primary needs to learn how to organize a project. For these sorts of situations, the best bet is (unfortunately) to have your experienced project leaders taking charge and ensuring that things happen correctly. If your team _only operates in this mode_, you are in crisis and need to make significant changes in order to ensure the long-term health and development of your team members. If you as a team member (whether junior or more senior) can't change things, go somewhere else -- another team, another company, whatever.

Finally, your team has to actually have an engineer with a lot of experience running projects to act as a secondary. If you don't have that kind of person on your team, then you would be far better off doing all planning and organizing as a full team. Have all of your discussions together, plan things out as a group, and do your best to learn and grow when things go wrong. If you do it all as a team, then you ensure that when things go wrong, no one person is left taking the blame (whether from the rest of the team or purely internalized). If possible, you can also seek mentors from other teams and find people who can help act as secondaries without being on the team with you. This winds up being closer to pure mentorship/advice-giving, but is better than nothing. 

## Examples

### Junior's First Big Feature

Junior engineers often spend the first year or two of their career working on well-defined tasks which are assigned to them by others on the team. These could be small project features (add a button, fix a style, record new data) or bug fixes. In these scenarios, someone else has done all of the planning necessary, so the junior's real task is to learn enough about this (hopefully) small issue and learn how to write code to handle it. Once our junior engineer has done this enough times, they will be ready to start learning the next steps:

* How can we break this bigger feature into smaller pieces?
* What order do those pieces need to be done in?
* How can we break those pieces up so that the work and code reviews are reasonably sized?
* What kind of feedback do I want from other people about the work I'm doing?
* What do I do when I encounter something that I didn't plan for?

In a primary/secondary environment, the junior engineer would be the primary on our feature, and a more experienced engineer would be the secondary. As the secondary, I'd be spending a lot of time pairing with the primary in order to explain how to break down the feature, how to plan for incremental delivery, estimating difficulty, etc. These skills are the work of an entire career to develop, but getting to pair directly with someone who has a _lot_ of experience with them can be a great way to jumpstart things for a junior. My experience has been that the explicit pairing and encouragement will help people feel safe to ask questions that they would not have otherwise felt safe to ask in a larger group (or even in a one-on-one). 

### Junior's First Big Feature With Others

By this point, it is expected that the junior engineer has delivered a few features, hopefully with the help of a good secondary to smooth things out. For this project, our junior engineer is working on their first project with one or more other more-junior engineers. In this case, the primary's responsibilities become a lot more complicated -- for the first time, they're in charge of setting up work for other engineers to do. A lot of the skills that juniors learn at this phase are essentially more-complex versions of those from earlier. Your primary will need to think about a number of new topics:

* What order do these tickets need to be done in order to keep everyone moving?
* How do we break work up so that the reviews are reasonably sized?
* What does the primary need to think about when providing review feedback for the other engineers?
* Who does the primary need to communicate with in terms of status updates for the project?
* What should the primary do when they realize that the project has encountered unexpected issues?

Leading a project for other engineers is a huge milestone in the careers of engineers, as it is a big step toward demonstrating that someone is ready to be a senior engineer. These sorts of projects are a lot more complicated, and there's a lot more to worry about. As the secondary, the responsibilities would be to guide the primary through all of these issues slowly, and to help them work through what's going on during the course of the project. At this stage of the primary's development, the secondary might still contribute code to solve particularly thorny blockers, but the emphasis would be much more heavily on the primary learning to recognize and solve these problems on their own. As a secondary in these situations, I would:

* Help the primary focus on ensuring that the other project participants always have work available
* Provide private feedback to the primary on any reviews they're giving to the other participants, in order to help them improve their reviewing skills
* Watch for signs that there are unplanned issues in the project, and try to call attention to them early (and in a psychologically safe way)
* Talk about the various aspects of what it means to be "responsible" for a project with other people working on it
* Help guide the primary learn about communicating with stakeholders

For projects like this, it is not an explicit goal of the secondary to prevent the primary from making mistakes. You can (and should) guide the primary toward possible issues, but it is an important step in the careers of younger developers to make mistakes and then learn from them. It _is_ an explicit goal that the secondary be ready to help the primary deal with the _consequences_ of these mistakes. This means helping the primary work through any possible imposter syndrome (again, in a psychologically safe way) while also providing guidance on what and how to communicate up to team leaders. The secondary should _not_ be informing product managers or engineering managers that things are going wrong unless the primary is completely failing to tell anyone that there are issues, because the goal is to ensure that the primary is learning how to manage expectations and reduce surprise for other team leads.

### Growing Seniors

As a staff engineer, one of my explicit responsibilities is to develop senior engineers into team leads or staff engineers. Primary/secondary also works great for this, because it provides a high touch (and private) way to work through more complex project planning. This can be a particularly difficult inflection point in the career of most senior engineers, because it's the point at which they will need to start growing the people around them. Rather than fixing every problem, people who want to be team leads have to learn how to teach their skills to others. They also have to come to terms with the fact that their day-to-day work on a project will include a larger focus on the _people_ working on a project, rather than purely on the code they write. As a senior primary working with a staff or lead secondary on a large project, you might expect to look at a quarter-long project and think about:

* What features does this project have?
* What order do the features need to be completed in?
* What are the major "unknown unknowns" that we might need to account for?
* What skillsets do we need at different points in the project? How will this impact other projects my team is doing?
* Who on the team can take on planning and executing the different features? Whose career goals or growth will be pushed forward?
* How do I help the engineers working on these projects?
* How do I give high quality review feedback that takes into account the needs of the project and the needs of the recipient?
* How do I anticipate problems? What are the warning signs?
* How do I communicate upward to more senior leads?

At this point in their career, the senior might be learning how to balance their own coding work with time spent as a secondary for the various features for their project. They will generally realize that in order to run the project well, they will need to hand off features that they might prefer to do themselves and instead be prepared to guide others through the implementation process. As the secondary for this senior engineer, you are (to some extent) guiding the senior engineer through the process of learning to be a good leader and secondary on all of the smaller parts of the project.
