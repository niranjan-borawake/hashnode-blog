## Coding Methods : Printer vs Pixel

Many of you may already be familiar with David's recent mini essay.
If you haven't read it already, you should do so before proceeding, and do follow [David](https://twitter.com/david_perell). 

%[https://twitter.com/david_perell/status/1445390288621539342?s=20]

These two writing methods could be used in coding as well.

Consider the fact that we need to develop a user account creation form.

Behaviour that is expected -

1. The user selects *Register*.
1. Completes the form by entering his or her name, email address, birth date, and phone number.
1. Selects *Get OTP* (One Time Password)
1. Enters the one-time password (OTP).
1. Submits the form.
1. A confirmation or error message is received.

Now we'll try to break down implementation of the above requirement using both the Printer and Pixel methods.

### Printer Method

Steps that one could take in this method include:

1. Add a *Register* button.
1. Create a form for *Account Creation* that includes all required fields.
1. Make the layout and design of the form pixel perfect.
1. Add form field validations and error messages.
1. Integrate with the Get OTP API.
1. Integrate with submit API.
1. Take care of the response message.
1. Develop unit tests

Steps 3 and 4 are time-consuming and, depending on the requirements, may become more complex. It's possible that you'll have to write and refactor code several times before the output from step 4 is perfect. Even if you have perfect code at the end of step 4, it will still not be functionally testable.

You'll still get the desired result, but it won't be as efficient as using the Pixel method.

Let's look at how the Pixel method differs.

### Pixel Method

This method can be broken down into three key steps:

1. Make it work.
1. Make it right.
1. Make it better.

Let's put these steps into practise with the example scenario.

#### Make it work.

1. Make a simple form with only required fields.
1. Get OTP by integrating with the API.
1. Integrate with submit API.
1. Develop unit tests

We don't worry about how perfect the code or the output is here; instead, we concentrate on the functionality. As a result, we have a functionally testable result at the end of this step. It has a lot of advantages to have core functionality working end-to-end as soon as possible.

#### Make it right

1. Include a *Register* button.
1. Implement the basic form layout
1. Add form field validations and error messages.
1. Handle the form submission response message 
1. Develop more unit tests.

We'll work on a few minor details that we purposefully overlooked in the previous step. We now have a fully implemented required behaviour at the end of this step.

#### Make it better

1. Create a pixel-perfect form layout.
1. Restructure and refactor code as needed.
1. Develop more unit tests.

This is where we put the finishing touches on the implementation.

I hope you found the Pixel method to be beneficial.

If you disagree or have suggestions for improving the Pixel method, please leave a comment below.

*Cover Photo from https://betterexplained.com/*


 

