## How did I fix a 2 week bug in 2 minutes

One of the well-known consulting firms was entrusted with a project. The agreement was that they would deliver us an MVP in three months, which we would then maintain and improve. I was given the task of overseeing the consultancy firm's development and delivery.

The consulting firm had assembled a strong team of about 15 people and was making good progress. The team consisted of a good mix of junior and senior developers.

One fine day, the Team Lead informed the Product Owners and Business that a specific feature would not be delivered as part of the MVP. The Product Owners and the Business, as expected, were not pleased. They contacted me to learn more about the situation. It came as a shock to me because the Team Lead had never discussed the problem with me.

I contacted Team Lead to learn more about the issue. He wasn't pleased, claiming that he had already discussed and brainstormed the problem with some of the other senior members several times. They had weighed all of the options and arrived at a decision. It will take at least two weeks to implement the solution if it is needed. With only one month until the MVP release, an extra two weeks was not an option. Later, he requested that one of the junior developers explain the problem to me.

The junior developer elaborated. I directed him to the documentation for one of the components that had caused the problem. After going over the documentation I asked him to add a new config to the component, test it, and ship it.

The problem was resolved in 2 minutes, not even 2 hours, despite the fact that it was supposed to take 2 weeks.

This emphasises the importance of carefully reading documentation.

### The issue 

The project had a small workflow of around 4-5 steps. These steps were designed as Tabs interface.


![Screenshot 2021-07-12 at 4.27.03 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1626087440830/dr2mFNGC6.png)

Step 1 required the user to upload multiple zip files, which could take up to a minute. The upload progress was indicated by a progress bar. The progress bar would lose its state if the user moved to another step in the workflow and then returned to Step 1.

Two options had been proposed by the team.

1. Disable any additional steps until the upload is complete.
  - This was obviously rejected by business.
1. 2 weeks to fix the progress bar state.
  - This required maintaining the progress state at a parent component, keeping it in sync with the upload progress as well as other complexities.


### Understanding the issue

The team had used a [react-tabs](https://reactcommunity.org/react-tabs/) component.

**Why did the progress bar lose its state?
**

In the react tabs component, the contents of only the *visible/active* Tab Panel (the area where input fields, progress bar is rendered) are kept in the *DOM* by default. As a result, when the user advances to Tab - *Step 2*, all of the content from Tab - *Step 1* is removed, and *Step 2* is added. When you switch tabs, the progress bar is unmounted and remounted and hence it loses its state.

### Solution

The *react-tabs* component already provides a config/prop [forceRenderTabPanel](https://github.com/reactjs/react-tabs#forcerendertabpanel-boolean). It is set to `false` by default but when set to `true` it renders all the Tab panel contents (active and inactive) all the time.

Now even if users moves to other tabs the progress bar status is updated correctly since the contents (including the progress bar) are always available in *DOM*.


> Instead of jumping to solutions right away, take your time to understand the problem.

Whether you're learning a new language, framework, or component, you should read the documentation carefully and thoroughly.

*Cover Photo by <a href="https://unsplash.com/@markuswinkler?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Markus Winkler</a> on <a href="https://unsplash.com/s/photos/magnifying-glass?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>*



