## Why and When to use React.useCallback?

During interviews, I've brought up `React.useCallback` several times. Almost everyone knew the answer to the question, "What is useCallback?"

But almost no one could explain -

- Why should you use it?
- When should it be used?
- How should it be used?

As per [React Docs](https://reactjs.org/docs/hooks-reference.html#usecallback) - 

#### useCallback

``` javascript
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```
> Returns a memoized callback.

> Pass an inline callback and an array of dependencies. `useCallback` will return a memoized version of the callback that only changes if one of the dependencies has changed. This is useful when passing callbacks to optimized child components that rely on reference equality to prevent unnecessary renders (e.g. `shouldComponentUpdate`).

The first part of the definition - 

> `useCallback` will return a memoized version of the callback that only changes if one of the dependencies has changed.

explains 

- What is `useCallback`?

Let's focus on the second part - 
> This is useful when passing callbacks to optimized child components that rely on reference equality to prevent unnecessary renders (e.g. `shouldComponentUpdate`).

There's a lot to take in and remember in this section. It provides answers to the following two questions.

- Why should you use it? -> to prevent unnecessary renders
- When should it be used? -> when passing callbacks to optimized child components

The documentation does not provide a detailed answer to the question - How should it be used?

Let's look at an example to see if we can grasp it.

### Defining a problem
We have a house with a playroom for the kids. Eat, watch TV, work, and sleep are all activities that parents engage in. Parents make child eat, play, and sleep. When the child is finished eating, playing, or sleeping, he or she should notify the parents. But the child is a child, and it cries every time a door of playroom is knocked.

Our goal is to stay as far away from knocking on doors as possible.

**Note: Re-rending kid's room (component) === Knocking its door
**
#### Step 1: A House with Parents who can eat, work, watch TV and sleep

``` javascript
function House () {
  const [parentAction, setParentAction] = React.useState('doing nothing');
  return (
    <section className="house">
      <label>House</label>
      <h1>Parents</h1>
      <div>
        <button onClick={() => setParentAction('eating')}>Eat</button>
        <button onClick={() => setParentAction('working')}>Work</button>
        <button onClick={() => setParentAction('watching TV')}>Watch TV</button>
        <button onClick={() => setParentAction('sleeping')}>Sleep</button>
      </div>      
      <div className="parents-action">Parents are {parentAction}.</div>
    </section>
  );
}
```
Try clicking on various actions Parents can perform.

<iframe width="100%" height="250" src="//jsfiddle.net/niranjan_borawake/ab3ju45q/embedded/result/dark/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

#### Step 2: A Kid's Room (shows number of times the door is knocked)

``` javascript
let kidRoomDoorKnockCounter = 0;
function KidRoom ({ kidAction = 'doing nothing' }) {
	return (
    <section className="kid-room">
      <label>Kid's Room</label>
      <div className="counter">Door Knock Counter: { ++kidRoomDoorKnockCounter }</div>
      <div className="kid-action">Kid is {kidAction}.</div>
    </section>
  );
}

```
Click on various actions of Parents to see door being knocked unnecessarily.

<iframe width="100%" height="400" src="//jsfiddle.net/niranjan_borawake/mcsqwn8b/embedded/result/dark/" allowfullscreen="allowfullscreen"  frameborder="0"></iframe>

#### Step 3: Fix unnecessary knocking of door

Let's optimize unnecessary re-renders by wrapping `KidRoom` component in [React.memo](https://reactjs.org/docs/react-api.html#reactmemo). `React.memo` will skip rendering the component if same `props` are `passed`. 

> Note: This is usually not required and we are doing this for the sake of the example.

```javascript
let kidRoomDoorKnockCounter = 0;
const KidRoom = React.memo(function KidRoom ({ kidAction = 'doing nothing' }) {
	return (
    <section className="kid-room">
      <label>Kid's Room</label>
      <div className="counter">Door Knock Counter: { ++kidRoomDoorKnockCounter }</div>
      <div className="kid-action">Kid is {kidAction}.</div>
    </section>
  );
});
```

Now we have an optimized child component which doesn't re-render when Parent actions change.

Revisit the definition of `useCallback` above. We are getting closer.

<iframe width="100%" height="400" src="//jsfiddle.net/niranjan_borawake/w4uxtep6/embedded/result/dark/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>
 
#### Step 4: Making kid eat, play and sleep

Parents should be able to make kid eat, play and sleep when required.

``` javascript
function House () {
  const [parentAction, setParentAction] = React.useState('doing nothing');
  // ------ Relevant code change for this step -----
  const [kidAction, setKidAction] = React.useState('doing nothing');
  const makeKid = (action) => {
  	setKidAction(action);
  };
 // ------------------------------------------------
  
  return (
    <section className="house">
      <label>House</label>
      <h1>Parents</h1>
      <div>
        <button onClick={() => setParentAction('eating')}>Eat</button>
        <button onClick={() => setParentAction('working')}>Work</button>
        <button onClick={() => setParentAction('watching TV')}>Watch TV</button>
        <button onClick={() => setParentAction('sleeping')}>Sleep</button>
      </div>      
      <div className="parents-action">Parents are {parentAction}.</div>
      {/*  ------ Relevant code change for this step ----- */}
      <h1>Make the Kid</h1>
      <div>
        <button onClick={() => makeKid('eating')}>Eat</button>
        <button onClick={() => makeKid('playing')}>Play</button>
        <button onClick={() => makeKid('sleeping')}>Sleep</button>
      </div>
      
      <KidRoom kidAction={kidAction} />
     {/*  ------------------------------------------------- */}
      
    </section>  
  );
}
```
Notice on each of this action the door is knocked. This is expected. You can't make a kid eat, play or sleep without knocking its door.

<iframe width="100%" height="400" src="//jsfiddle.net/niranjan_borawake/tay8q4nr/embedded/result/dark/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

#### Step 5: Kid should notify Parents when done

``` javascript
let kidRoomDoorKnockCounter = 0;
const KidRoom = React.memo(function KidRoom ({ kidAction = 'doing nothing', iAmDone }) {
	return (
    <section className="kid-room">
      <label>Kid's Room</label>
      <div className="counter">Door Knock Counter: { ++kidRoomDoorKnockCounter }</div>
      <div className="kid-action">Kid is {kidAction}.</div>
       {/*  ------ Relevant code change for this step ----- */}
      <button disabled={kidAction === 'doing nothing'} onClick={iAmDone}>I'm Done</button>
       {/*  ------------------------------------------------- */}
    </section>
  );
});

function House () {
  const [parentAction, setParentAction] = React.useState('doing nothing');
  const [kidAction, setKidAction] = React.useState('doing nothing');
  // ------ Relevant code change for this step -----
  const [isKidDone, setIsKidDone] = React.useState(true);
  const iAmDone = () => {
    setIsKidDone(true);
    setKidAction('doing nothing');
  };
 // -------------------------------------------------
  const makeKid = (action) => {
    setKidAction(action);
    setIsKidDone(false);
  };
  return (
    <section className="house">
      <label>House</label>
      <h1>Parents</h1>
      <div>
        <button onClick={() => setParentAction('eating')}>Eat</button>
        <button onClick={() => setParentAction('working')}>Work</button>
        <button onClick={() => setParentAction('watching TV')}>Watch TV</button>
        <button onClick={() => setParentAction('sleeping')}>Sleep</button>
      </div>      
      <div className="parents-action">Parents are {parentAction}.</div> 
      <h1>Make the Kid</h1>
      <div>
        <button disabled={!isKidDone} onClick={() => makeKid('eating')}>Eat</button>
        <button disabled={!isKidDone} onClick={() => makeKid('playing')}>Play</button>
        <button disabled={!isKidDone} onClick={() => makeKid('sleeping')}>Sleep</button>
      </div> 
       {/*  ------ Relevant code change for this step ----- */}
      <KidRoom kidAction={kidAction} iAmDone={iAmDone} /> 
       {/*  ------------------------------------------------- */}
    </section>
  );
}
```
Now kid can notify parents when it's done. Also, parents cannot force their kid to do something else until he or she is finished with what it's doing.

<iframe width="100%" height="450" src="//jsfiddle.net/niranjan_borawake/ou4r863f/embedded/result/dark/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Everything appears to be as expected. But wait, have you tried clicking on the different actions that Parents can take? If not, give it a shot.

Why do they knock on the door every time they do something? The kid is getting completely insane.

We have already wrapped `KidRoom` in `React.memo`. The same `props` are being sent when the Parent's action change. Why unnecessary re-renders again?

Let's go back to the second part of the definition.
> This is useful when passing callbacks to optimized child components that rely on reference equality to prevent unnecessary renders (e.g. `shouldComponentUpdate`).

Hmm. That's exactly what we are doing. We are passing a callback - `iAmDone` to an optimized child component - `KidRoom`. 

> that rely on reference equality to prevent unnecessary renders

What's this?

Every time `House` component is rendered, `iAmDone` callback gets a new reference which means `KidRoom` gets a new `prop`. `React.memo` won't optimize if it receives a new `prop`.

Now, let's go to the first part of the definition and get a `memoized` callback with `useCallback` so that we get the same reference (reference equality) of `iAmDone`.

> If a new reference is expected when some values change then pass those values as an array of dependencies in second argument to `useCallback`.

#### Step 6: `useCallback` in action

``` javascript
function House () {
  const [parentAction, setParentAction] = React.useState('doing nothing');
  const [kidAction, setKidAction] = React.useState('doing nothing');
  const [isKidDone, setIsKidDone] = React.useState(true);
  
  const iAmDone = React.useCallback(() => {
  	setIsKidDone(true);
    setKidAction('doing nothing');
  }, []);
  
  //---- rest of the code remains same
}
```

KidRoom does not re-render when parents take different actions.

<iframe width="100%" height="450" src="//jsfiddle.net/niranjan_borawake/ahkm0zuv/embedded/result/dark/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

Finally, we were able to keep the child from being bothered by unnecessary knocks on the door.

I hope this helped you understand W3H - What | Why | When | How to use `React.useCallback`.

### References
- https://reactjs.org/docs/hooks-reference.html#usecallback
- https://reactjs.org/docs/react-api.html#reactmemo
- https://kentcdodds.com/blog/usememo-and-usecallback

*Cover Photo by <a href="https://unsplash.com/@igorstarkoff?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Igor Starkov</a> on <a href="https://unsplash.com/s/photos/kid-room?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>*
  












