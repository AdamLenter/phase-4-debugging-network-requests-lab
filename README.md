# Putting it All Together: Client-Server Communication

## Learning Goals

- Understand how to communicate between client and server using fetch, and how
  the server will process the request based on the URL, HTTP verb, and request
  body
- Debug common problems that occur as part of the request-response cycle

## Introduction

Just like the last lesson, we've got code for a React frontend and Rails API
backend set up. This time though, it's up to you to use your debugging skills to
find and fix the errors!

To get the backend set up, run:

```console
$ bundle install
$ rails db:migrate db:seed
$ rails s
```

Then, in a new terminal, run the frontend:

```console
$ npm install --prefix client
$ npm start --prefix client
```

Confirm both applications are up and running by visiting
[`localhost:4000`](http://localhost:4000) and viewing the list of toys in your
React application.

## Deliverables

In this application, we have the following features:

- Display a list of all the toys
- Add a new toy when the toy form is submitted
- Update the number of likes for a toy
- Donate a toy to Goodwill (and delete it from our database)

The code is in place for all these features on our frontend, but there are some
problems with our API! We're able to display all the toys, but the other three
features are broken.

Use your debugging tools to find and fix these issues.

There are no tests for this lesson, so you'll need to do your debugging in the
browser and using the Rails server logs and `byebug`.

**Note**: You shouldn't need to modify any of the React code to get the
application working. You should only need to change the code for the Rails API.

As you work on debugging these issues, use the space in this README file to take
notes about your debugging process. Being a strong debugger is all about
developing a process, and it's helpful to document your steps as part of
developing your own process.

## Your Notes Here

- Add a new toy when the toy form is submitted

  - How I debugged:
  I tried to add a toy. Then I opened my console and saw an error that each child in a list needs a unique key. I recognized that as a React error. So I went in to the React code and saw that each toy SHOULD have an key. So then I looked at the list of toys and saw that my toy had been added without a name or a number of likes. 
  
  I didn't want that bad entry in the list. So I went into my Rails console to delete it. But I discovered it hadn't actually been created. Therefore, the app must have added a toy to state without actually adding it to the database.

  Time to troubleshoot the POST request...

  I replaced the create method with a byebug. I tried to enter my toy again. I went into my byebug console and looked at params. The data looked good.

  So I looked at the next line of code and saw Toys.create. I changed that to Toy.create and tried again.

  Yikes - the toy didn't appear on the list. So I reloaded the page. Yikes - all the toys were gone. In my browser console window, there was an error in my GET request. What's up with that? I had a feeling there was something wrong with the toy i just added. So I went back into the Rails console to check on it...

  Nope - my toy wasn't even on the list. Huh? So what happened to all the toys?
  
  Oops - wait - I had accidentally stopped my rails server. Started it again. There are the toys, but my new one isn't there.

  I added a "rescue from" to my toys controller for the event that there is an error in creating the toy. 

  Now it was created just fine. Hmmm... I guess accidentally stopping the server was the big mistake.

  I created a new toy. It's fine. So I wasted 10 mins there. My bad.

- Update the number of likes for a toy

  - How I debugged:

  I clicked the "like" button and got a React error. So I went back and reloaded the page. The page reloaded fine and the like was there. It looks like something is wrong with the response. I hadn't actually read the error message. So I went back and replicated the error.
  Here's the error: "Unexpected end of json input."

  I glanced at the React code. It looks good. So there must be an issue with the Fetch response. I looked at the toys controller. In fact, there was no response being sent.

  I added a response and tested it. Looks good.



- Donate a toy to Goodwill (and delete it from our database)

  - How I debugged:
  First I clicked the link. I received a 404 - not found. I reloaded the page and the toy was still there.

  I checked the routes and saw that destroy wasn't on the list. I added it. Bada bing bada boom