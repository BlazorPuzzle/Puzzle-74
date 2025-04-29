# Puzzle #74 - Canceling Navigation

Jeff and Carl want to know how to interrupt a navigation request and optionally cancel it in a .NET 9 Blazor Web App with Global Server interactivity.

YouTube Video: https://youtu.be/UkHezqeIQUE

Blazor Puzzle Home Page: https://blazorpuzzle.com

## The Challenge

This is a .NET 9 Blazor Web App with Global Server interactivity.

When the user navigates away from this page, we want to be notified and also be able to cancel the navigation.

You might need this if, for example, the user is adding a new record. You could ask the user to confirm whether they really want to navigate away from the page before saving.

We know we need a Navigation Manager, but how can we use it to cancel the navigation?

## The Solution

Rather than using the `LocatioChanged` event, the solution is to call `RegisterLocationChangingHandler` passing a delegate to a handler method where we can call `PreventNavigation()`.

We need a disposable variable to store the handler:

```c#
private IDisposable? navigationHandler;
```

Here's an example of the handler method:

```c#
private async ValueTask OnLocationChanging(LocationChangingContext context)
{
    // Example: show a confirmation dialog
    var shouldLeave = await jsRuntime.InvokeAsync<bool>("confirm", "You have unsaved changes. Are you sure you want to leave?");
    if (!shouldLeave)
    {
        context.PreventNavigation(); // Cancels the navigation
    }
}
```

Now we just need to wire it up in `OnInitialized()` and dispose the handler in `Dispose()`:

```c#
protected override void OnInitialized()
{
    navigationHandler = navigationManager.RegisterLocationChangingHandler(OnLocationChanging);
}

public void Dispose()
{
    // Unsubscribe from the handler when the component is disposed
    navigationHandler?.Dispose();
}
```

Boom!