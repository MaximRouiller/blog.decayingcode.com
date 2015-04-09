---
title: "Model View Presenter Revisited"
date: 2009-02-23 22:40:00
tags: [architecture]
---

The [MVP pattern](http://en.wikipedia.org/wiki/Model_View_Presenter) is a pattern that came into being in the early 1990s by [Taligent](http://en.wikipedia.org/wiki/Taligent). This pattern is mostly used inside WinForms and WebForms.

The View normally don't do anything. The official implementation is described as the following.

> The view instantiate the presenter with an instance of itself. The constructor parameter of the presenter must be an interface of the view. When events of the view happens, they must call the presenter without any parameter/return value. If the presenter need data, the presenter will get the data from the view interface without the view giving the data directly. Changes to the view must be done through the presenter.

Of course, this is a literal implementation from 1990\. Of course, today we have more advanced paradigm that works quite nice. What is interesting is, with proper data binding, that we can change the value on the views without even calling methods of the view.

It is possible to add a databinding on a property of the presenter. Once the databinding is done, it's possible to just change the presenter's property to fire events on the view that will automatically updates the control.

It removes some implementation details of the MVP and make the pattern easier to implement.

Need a sample? Here it goes to implement an "auto-notify" property when it change inside a presenter:

```cs
public class MyPresenter : IPresenter, INotifyPropertyChanged
{
    private readonly IView view;
    private int randomNumber;

    public int RandomNumber
    {
        get { return randomNumber; }
        set
        {
            if (randomNumber == value) return;

            randomNumber = value;
            RaiseEvent("RandomNumber");
        }
    }

    #region Implementation of INotifyPropertyChanged

    // custom method to ease the change of event
    public void RaiseEvent(string propertyName)
    {
        PropertyChanged(this, new PropertyChangedEventArgs(propertyName));
    }

    public event PropertyChangedEventHandler PropertyChanged = delegate { };

    #endregion

    public MyPresenter(IView view)
    {
        this.view = view;
        this.view.InitializeBindings(this);
    }

    public void GenerateRandomNumber()
    {
        Random rnd = new Random(DateTime.Now.Millisecond);
        RandomNumber = rnd.Next(0, 100);
    }
}
```

This will raise an event every time that a DIFFERENT value will be assigned to the property **RandomNumber** or when the event is fired. Now for the view, it look like this:

```cs
public partial class frmMain : Form, IView
{
    private readonly IPresenter presenter;

    public frmMain()
    {
        InitializeComponent();
        presenter = new MyPresenter(this);
    }

    public void InitializeBindings(IPresenter currentPresenter)
    {
        textBox1.DataBindings.Add("Text", currentPresenter, "RandomNumber", false, DataSourceUpdateMode.Never);
    }

    private void button1_Click(object sender, EventArgs e)
    {
        presenter.GenerateRandomNumber();
    }
}
```

The method `InitializeBindings` is called in the constructor of the presenter and will ensure that the binding are made only once. This will **NOT** require additional methods inside the view to assign the generated number inside the TextBox. This implementation respect the model definition while using the latest binding technology of .NET.

This reduce the amount of useless method while keeping the framework in charge of the bindings.

Here is the resulting interfaces from the implementation:

```cs
public interface IPresenter
{
    int RandomNumber { get; set; }
    void GenerateRandomNumber();
}

public interface IView
{
    void InitializeBindings(IPresenter currentPresenter);
}
```