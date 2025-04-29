# Puzzle #74 - Canceling Navigation

Jeff and Carl want to know how to interrupt a navigation request and optionally cancel it in a .NET 9 Blazor Web App with Global Server interactivity.

YouTube Video: https://youtu.be/UkHezqeIQUE

Blazor Puzzle Home Page: https://blazorpuzzle.com

## The Challenge

This is a .NET 9 Blazor Web App with Global Server interactivity.

When the user navigates away from this page, we want to be notified and also be able to cancel the navigation.

You might need this if, for example, the user is adding a new record. You could ask the user to confirm whether they really want to navigate away from the page before saving.

We know we need a Navigation Manager, but how can we use it to cancel the navigation?