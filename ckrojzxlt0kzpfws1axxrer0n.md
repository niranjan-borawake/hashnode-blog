## How reading code (rather than just writing) helped me advance in my career

Coding is the ability to *read* and write code. However, as a beginner, the majority of the emphasis has always been on being able to write code and very little on reading.

> Indeed, the ratio of time spent reading versus writing is well over 10 to 1. We are constantly reading old code as part of the effort to write new code. ...[Therefore,] making it easy to read makes it easier to write.

> ― Robert C. Martin, Clean Code: A Handbook of Agile Software Craftsmanship

If the ration is 10:1, we better understand how to read code.

### What does *reading code* mean?

<center>
![Reading Code](https://media1.tenor.com/images/aa4370a44a2b3edc4c7c7a2d2c2005f8/tenor.gif?itemid=7949165)
</center>

Reading code is all about reading code 😬. It's just like reading a book. If a paragraph on a page does not make sense to us, we reread it until we do. It's no different when it comes to reading code. We read code to better understand it.

A given piece of code could be read in a variety of ways.
- For some it may be simple to read from the point where the execution begins and follow the path.
- For others it may be easier to read each smaller module first and then understand how they interact with one another.

Also, reading code and debugging, in my opinion, are two entirely different things. To better understand code, one can use debugging as a tool while reading code.

### Enough talk
<center>
![What's your story](https://media1.tenor.com/images/224aa55758c164c55e1647327d82f69f/tenor.gif?itemid=20067189)
</center>

I had joined a UI Component Library team. Soon after, I was assigned a bug involving a Data Table component.

*Note, all the components were custom built.*

One of the features of Data Table was [aggregation](https://www.ag-grid.com/javascript-grid/aggregation/) where rows are grouped based on some grouping logic.

<center>
![aggregation.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627471196028/LlwqRdDPR.png)
</center>


In the documentation site (on aggregation page) there were many aggregation examples demoing various features like sorting, filtering, row selection, etc. 

### The Bug

If we expand a row *United States* in a sorting example it expands the same row in filtering and all other examples as well. Ideally each Data Table should have its own state to record if the row is `expanded`. Expanding a row in one table should not expand the row in some other table.

Please keep in mind that I was new to the Data Table component, the component library, the team, and the firm.

First, I went over the library, the demo site, the Data table component, as well as its plugins.

I then debugged the demo site and the aggregation plugin. 

My observations - 

- The `expanded` state of the row is being saved as part of the row data fed to table.
- All of the Data Table examples on this page use the same data variable (reference).

Sample data. Note the `__expanded` flag set to `true`.

```javascript 
[
  {
    id: '12345',
    country: 'United States'
    min: '15',
    max: '49',
    children: [ {
      year: '2008',
      ...
    }],
   __expanded: true,
   ...
  }, {
    ...
  }
]
```

The issue was crystal clear to me. Because the same data variable is passed to all Data Table examples, the `expanded` state of rows is shared.

I began to consider possible solutions:
- For each Table in the demo site, pass a different data variable. (Avoiding a problem does not imply that it has been resolved.)
- Separate the row's meta data (`expanded` state) from the actual data. Create a mapper that can map a row to the `expanded` state based on `id`.

### The Fix

I resisted the urge to write any code.
<center>
![Can't do that](https://media1.tenor.com/images/e3e3135a2753b98e54f80786b69b2316/tenor.gif?itemid=15989587)
</center>

I convinced myself that I was still incapable of writing code to fix a bug in the library's most complex component. I slept on it and then began *reading code* the next day.

I learned about other plugins and experimented with different features. I attempted to understand those plugins.

I concentrated on the Row Selection feature. There were multiple examples. Selecting a single row, multiple rows, and so on. I selected the first row of the first table. To my surprise, the first row of each of the remaining tables had not been selected. I quickly checked to see if all of the examples used the same data variable. They didn't. Each table was given its own `data` variable. Without further ado, I changed the documentation code locally to use the same `data` variable in all of the examples. This time, I expected the first row of every table to be selected as soon as I selected the first row of the first table. `expanded` or `selected` they are both different states of the table and should behave similarly. But the Data Table surprised me once more. All of the first rows in all of the tables were *not* selected. This means that a row's `selected` state is not shared between tables. I learned more about the selection plugin by *reading more code*.

Observations/ Findings:
- In contrast to `expanded`, `selected` state was not stored in row data.
- Instead, the table already had a separate plugin to manage/map various states such as `selected`, `disabled`, and so on to each row based on row `id`.

### Impact of reading code

I sent an email to the entire team. I asked a simple question: why haven't we used the row mapper plugin to manage the `expanded` state of a row, as we have in other plugins to manage the `selected` and `disabled` states?

The next day, almost everyone on the team (especially the seniors) appreciated my question and agreed that was the best way to proceed.

Some of the replies - 

Reply 1:

> Hey Niranjan, where were you on this before? 

Reply 2:
> You nailed it. That's the way to go.

Reply 3:

> Nearly a year ago, I wrote the row mapper plugin. We've all been so preoccupied with other new components that we've completely forgotten about it. You are entirely correct. In fact, the plugin was created for the same purpose.

Let's understand both the scenarios - 

#### 1. Immediately writing code.
- I think I could have fixed the bug in 2-3 days by extracting out the `expanded` state from data and introducing some mapping logic.
- I may have received some PR feedback.
- I may have come across as a doer.
- I would have been given a new bug to work on.

#### 2. Reading code before writing.

Instead, what happened was -

- My question was well received by the team.
- In my early days, I drew the attention of the entire team.
- The team thought of me as someone who reads and understands code.
- I was immediately tasked with creating a completely new Table plugin.
- I was declared the owner of the Data Table component after only a few months. I was in charge of designing and reviewing all future features/plugins.

That's the end of the story.

My advice to beginners is to develop the habit of reading code. If you're participating in #100daysofcode, set aside a few days to learn how to *read code*. Visit some open source projects and attempt to read the code. It is also a useful skill to have when reviewing PRs. If you can quickly read and understand code, you'll be great at reviewing PRs.

Finally,

> Always read before you write.

I wish you all the best with your code reading.

*Cover Photo by <a href="https://unsplash.com/@lunarts?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Volodymyr Hryshchenko</a> on <a href="https://unsplash.com/s/photos/growth?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>*