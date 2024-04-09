# How to use custom data to style your TalkJS chat

You might often have situations where you want to style your chat differently for different users. Creating a theme for different subset of users is not feasible. TalkJS gives you the option to pass custom data to the theme that you can use to style your chat.

<figure class="kg-image-card">
  <img class="kg-image" src="1-custom-styled-chats.png" alt="Styling chats differently using custom data in the same theme."/>
  <figcaption>You can style chats using custom data without creating multiple themes.</figcaption>
</figure>

In this tutorial, we're going to showcase three chats each with a different `accentColor` passed to the theme and rendered differently. The following sections highlight the main topics that we'll cover:

* Editing the HTML layout to add three chatboxes side-by-side
* Passing custom data to the chatboxes
* Using the custom data in the TalkJS theme

To follow along, you’ll need:

A [TalkJS account]|(https://talkjs.com/dashboard/login). TalkJS provides a ready-to-use chat client for your application. Your account gives you access to TalkJS's free development environment.
An existing TalkJS project using the [JavaScript Chat SDK](https://talkjs.com/docs/Reference/JavaScript_Chat_SDK/). See our [Getting Started]|(https://talkjs.com/docs/Getting_Started/) guide for an example of how to set this up.

We’ll build up the feature step by step in the following sections. If you would rather see the complete example code, see the [GitHub repo]() for this tutorial. <!-- Add Github link -->

## Editing the HTML layout to add three chatboxes side-by-side

If you followed our [Getting Started](https://talkjs.com/docs/Getting_Started/) guide, you'll have a single div with the id `talkjs-container`. There are also some inline styles applied to this div. You must replace this HTML code with the block given below.

```html
<div class="container">
  <div id="talkjs-container-1" class="column">
    <i>Loading chat...</i>
  </div>
  <div id="talkjs-container-2" class="column">
    <i>Loading chat...</i>
  </div>
  <div id="talkjs-container-3" class="column">
    <i>Loading chat...</i>
  </div>
</div>
```

### Moving the inline styles to a stylesheet

Notice how we've moved the inline styles out into a stylesheet. For this, you must create a `styles.css` file in the project directory and paste the following styles. If you cloned the Github project, you can skip this step.

```css
.container {
    display: flex;
    justify-content: space-between;
    height: 500px;
    margin: 30px;
}
.column {
    flex: 1;
}
```

### Setting the theme for your chat

Go to your TalkJS dashboard, and navigate to the **Chat UI** tab. Then, select the default role and select the **customDataToTheme** theme. Click **Publish** to live to apply this theme to your default role.

## Passing custom data to the chatboxes

Inside the `index.js` file, we're creating 4 users in total. It is to demonstrate one user talking to three other users.
```javascript
const me = new Talk.User({
id: "0001",
name: "Mikaela Ross",
email: "mikaela.ross@example.com",
photoUrl: "https://talkjs.com/images/avatar-7.jpg",
role: "default"
});

const other1 = new Talk.User({
id: "0002",
name: "Thomas River",
email: "thomas.river@example.com",
photoUrl: "https://talkjs.com/images/avatar-5.jpg",
role: "default"
});

const other2 = new Talk.User({
id: "0003",
name: "Kirsten Doe",
email: "kirsten.doe@example.com",
photoUrl: "https://talkjs.com/images/avatar-1.jpg",
role: "default"
});

const other3 = new Talk.User({
id: "0004",
name: "James Fayland",
email: "james.fayland@example.com",
photoUrl: "https://talkjs.com/images/avatar-4.jpg",
role: "default"
});
```

After creating 4 users, we create three conversations. We set the participants for each of these conversations. 

```javascript
let conversation1 = talkSession.getOrCreateConversation("customStyleChat1");
let conversation2 = talkSession.getOrCreateConversation("customStyleChat2");
let conversation3 = talkSession.getOrCreateConversation("customStyleChat3");

conversation1.setParticipant(me);
conversation1.setParticipant(other1);

conversation2.setParticipant(me);
conversation2.setParticipant(other2);

conversation3.setParticipant(me);
conversation3.setParticipant(other3);
```

Next, we create three chatboxes and [pass data to the theme](https://talkjs.com/docs/Features/Themes/Passing_Data_to_Themes/). The variable name `accentColor` is something we've used in this example, but you can change it to whatever makes sense for you.

```javascript
const chatbox1 = talkSession.createChatbox({
  theme: {
    custom: {
      accentColor: '#EE4B2B',
    },
  },
});

const chatbox2 = talkSession.createChatbox({
  theme: {
    custom: {
      accentColor: '#E69597',
    },
  },
});

const chatbox3 = talkSession.createChatbox({
  theme: {
    custom: {
      accentColor: '#56AE57',
    },
  },
});
```

Lastly, we select the created conversations and mount the three chatboxes.

```javascript
chatbox1.select(conversation1);
chatbox2.select(conversation2);
chatbox3.select(conversation3);
  
chatbox1.mount(document.getElementById("talkjs-container-1"));
chatbox2.mount(document.getElementById("talkjs-container-2"));
chatbox3.mount(document.getElementById("talkjs-container-3"));
```

## Using the custom data in the TalkJS theme

Go to your TalkJS dashboard, and navigate to the **Themes** tab. Click **Edit** next to the **customDataToTheme** theme. Go to the "UserMessage" component and find the `.by-me .message` CSS class. Change the values of the `border-color` and `background-color` properties to `var(--theme-accentColor)`.

Now, go to the "MessageField" component and find the following CSS class.

```css
.is-typing .send-button,
.record-button:hover,
.location-button:hover,
.attachment-button:hover,
.emoji-button:hover
```

Change the value of the `color` property to `var(--theme-accentColor);`.

Lastly, go to the "ChatHeader" component and find the `header` CSS class. Change the value of the `background-color` CSS property to `var(--theme-accentColor)`. TalkJS autosaves all your changes automatically.

## Conclusion

<figure class="kg-image-card">
  <img class="kg-image" src="2-demo.gif" alt="Each chat is styled differently based on the data passed to the theme."/>
  <figcaption>Each chat is styled differently based on the data passed to the theme.</figcaption>
</figure>

You now know how to use custom data to style your TalkJS chat. To recap, in this tutorial you have:

- Passed custom data while creating chatboxes
- Accessed the custom data in your TalkJS theme
- Style different components using the custom data

For the full example code for this tutorial, see our [GitHub repo]().

If you want to learn more about TalkJS, here are some good places to start:

- The [TalkJS Docs](https://talkjs.com/docs/) help you get started with TalkJS.
- [TalkJS tutorials](https://talkjs.com/resources/tag/tutorials/) provide how-to guides for many common TalkJS use cases.
- The [talkjs-examples Github repo](https://github.com/talkjs/talkjs-examples) has larger complete examples that demonstrate how to integrate with other libraries and frameworks.