---
title: "Easily enable databindings on a ToolStripButton"
date: 2009-02-11 23:53:00
tags: [c#]
---

I was developping an application lately and I needed to bind the "Enabled" property of a ToolStripButton to my Presenter. I failed to find any "DataSource" or "DataBindings" property. I then decided to make my own button without reinventing the wheel to enable this capability.

Here's this simple class:

```cs
    public class ToolStripBindableButton : ToolStripButton, IBindableComponent
    {
        private ControlBindingsCollection dataBindings;

        private BindingContext bindingContext;

        public ControlBindingsCollection DataBindings
        {
            get
            {
                if(dataBindings == null) dataBindings = new ControlBindingsCollection(this);
                return dataBindings;
            }
        }

        public BindingContext BindingContext
        {
            get
            {
                if(bindingContext == null) bindingContext = new BindingContext();
                return bindingContext;
             }
            set { bindingContext = value; }
        }
    }
```

Once you include this simple class inside your project/solution... you can easily convert any **ToolStripButton** into our new **ToolStripBindableButton**.

And I solved my problem like this:

```cs
myBindableButton.DataBindings.Add("Enabled", myPresenter, "CanDoSomething");
```
