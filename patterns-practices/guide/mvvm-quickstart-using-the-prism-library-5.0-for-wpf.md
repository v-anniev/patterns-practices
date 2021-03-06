---
TOCTitle: 'MVVM QuickStart Using the Prism Library 5.0 for WPF'
Title: 'MVVM QuickStart Using the Prism Library 5.0 for WPF'
ms:assetid: '2852cec9-d4d3-4961-9ed5-c3edec0ab05f'
ms:mtpsurl: 'https://msdn.microsoft.com/en-us/library/Gg430857(vPandP.40)'
---

# MVVM QuickStart Using the Prism Library 5.0 for WPF

From: [Developer's Guide to Microsoft Prism Library 5.0 for WPF](/patterns-practices/index)

The Model-View-ViewModel (MVVM) QuickStart provides sample code that demonstrates how to separate the state and logic that support a view into a separate class named **ViewModel** using the Prism Library. The view model sits on top of the application data model to provide the state or data needed to support the view, insulating the view from needing to know about the full complexity of the application. The view model also encapsulates the interaction logic for the view that does not directly depend on the view elements themselves. This QuickStart provides a tutorial on implementing the MVVM pattern.

A common approach to designing the views and view models in an MVVM application is the first sketch out or storyboard for what a view looks like on the screen. Then you analyze that screen to identify what properties the view model needs to expose to support the view, without worrying about how that data will get into the view model. After you define what the view model needs to expose to the view and implement that, you can then dive into how to get the data into the view model. Often, this involves the view model calling to a service to retrieve the data, and sometimes data can be pushed into a view model from some other code such as an application controller.

This QuickStart leads you through the following steps:

-   Analyzing the view to decide what state is needed from a view model to support it
-   Defining the view model class with the minimum implementation to support the view
-   Defining the bindings in the view that point to view model properties
-   Attaching the view to the view model

## Business Scenario

The main window of the Basic MVVM Application QuickStart represents a subset of a survey application. In this window, an empty survey with different types of questions is shown; and there is a button to submit the questionnaires. The following illustration shows the QuickStart main window.

![](images/mvvm_quickstart-user-interface.png "MVVM QuickStart user interface")

MVVM QuickStart user interface

## Building and Running the QuickStart

This QuickStart requires Microsoft Visual Studio 2012 or later and the .NET Framework 4.5.1.

**To build and run the MVVM QuickStart**

1.  In Visual Studio, open the solution file Quickstarts\\BasicMVVMQuickstart\_Desktop\\BasicMVVMQuickstart\_Desktop.sln.
2.  In the **Build** menu, click **Rebuild Solution**.
3.  Press F5 to run the QuickStart.

## Implementation Details

The QuickStart highlights the key elements and considerations to implement the MVVM pattern in a basic application.

## Analyzing What Properties Are Needed on the View Model

Open the Views\\MainWindow view in the designer. The list of color selections will be dynamically populated. When analyzing a view to define a view model, you want to identify each individual item of data and behavior that you need. In this case, assuming the questions are fixed and will not be dynamically driven, you need the following properties exposed from your view model:

-   Name: string
-   Age: int
-   Quest: string
-   FavoriteColor: string
-   Submit: Command
-   Reset: Command

Because the first four properties are related to questionnaires, a questionnaire class is created to store them. The questionnaire class will be the model of the application, and the view model will only expose a property of type **Questionnaire** to support them.

Note that even things like buttons represent something that needs support from the view model. You can either expose a command, as shown in this QuickStart, or you can expose a method. With the former, you will need a property exposed from the view model with an object that implements the **ICommand** interface; with the latter, you need a behavior that can target a method.

> [!NOTE]
> For button clicks, you have the choice of commands or behaviors. For more information, see [Command-Enabled Controls vs. Behaviors](/patterns-practices/guide/6-advanced-mvvm-scenarios-using-the-prism-library-5.0-for-wpf#CommandEnabledControls) in [Advanced MVVM Scenarios](/patterns-practices/guide/6-advanced-mvvm-scenarios-using-the-prism-library-5.0-for-wpf). In this topic, you will use a command. To do that, you need a command implementation, which does not exist in a form compatible with view models in the .NET Framework. Prism provides the **DelegateCommand** class that is perfect for hooking up views to view models with commands.

As we want to demonstrate parent-child view model composition, the application UI is composed by two views: **MainWindow**, which contains the **Reset** and **Submit** buttons and an instance of the second view, which is the **QuestionnaireView** that includes the questionnaire's questions.

The **QuestionnaireView** is directly instantiated in the XAML code, as seen in the following code.

```XAML
    <Window x:Class"BasicMVVMQuickstart_Desktop.Views.MainWindow" ...>
        <Grid x:Name"LayoutRoot"
              Background"{StaticResource MainBackground}">
            <Grid MinWidth"300" MaxWidth"800">
                ...
                <views:QuestionnaireView Grid.Row"1"
                                         DataContext"{Binding QuestionnaireViewModel}"
                                         Height"246" VerticalAlignment"Top">
                </views:QuestionnaireView>
                ...
            </Grid>
        </Grid>
    </Window>
```

In order to populate this child view with its corresponding view model, its **DataContext** is set to a property of the **MainWindow's** view model that contains an instance of the child **QuestionnaireView's** view model.

## Implementing the View Model to Support the View

Open the QuestionnaireViewModel.cs file. The view model implements the Questionnaire and **AllColors** properties and derives from the **BindableBase** class.

```C#
    public class QuestionnaireViewModel : BindableBase
    {
        private Questionnaire questionnaire;
        public QuestionnaireViewModel()
        {
            this.Questionnaire  new Questionnaire();
            this.AllCollors  new[] { “Red”, “Blue”, “Green” };
        }

        public Questionnaire Questionnaire
        {
            get { return this.questionnaire; }
            set { SetProperty(ref this.questionnaire, value); }
        }
        public IEnumerable<string> AllCollors { get; private set; }
    }
```

The **INotifyPropertyChanged** interface is implemented on the **BindableBase** base class. The property changed notification is added to the whole **Questionnaire** property, using the **SetProperty** method of the **BindableBase** class as shown in the following code.

```C#
    public Questionnaire Questionnaire
    {
        get { return this.questionnaire; }
        set { SetProperty(ref this.questionnaire, value); }
    }
```

> [!NOTE]
> The view model class typically derives from the **BindableBase** class. In some cases, the model can derive from **BindableBase**, when the property that needs to update the view when its value is changed is stored in the model.

To support **INotifyPropertyChanged**, your class needs to derive from the **BindableBase** class, and the property setter needs to call the **SetProperty** method of the **BindableBase** class.

The following code shows how the **BindableBase** class implements the **INotifyPropertyChanged** interface. Note that the **SetProperty** method updates the property’s value and fires the **PropertyChanged** event when a property changes its value. Alternatively, you can use the **OnPropertyChanged** method, passing a lambda expression that references the property, to fire the **PropertyChanged** event. This is useful for when one property update triggers another property update. And also provides backward compatibility with the previous version of Prism.

```C#
    public abstract class BindableBase : INotifyPropertyChanged
    {
        public event PropertyChangedEventHandler PropertyChanged;

        protected virtual bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName  null)
        {
            if (object.Equals(storage, value)) return false;

            storage  value;
            this.OnPropertyChanged(propertyName);

            return true;
        }

        protected void OnPropertyChanged(string propertyName)
        {
            var eventHandler  this.PropertyChanged;
            if (eventHandler ! null)
            {
                eventHandler(this, new PropertyChangedEventArgs(propertyName));
            }
        }

        protected void OnPropertyChanged<T>(Expression<Func<T>> propertyExpression)
        {
            var propertyName  PropertySupport.ExtractPropertyName(propertyExpression);
            this.OnPropertyChanged(propertyName);
        }
    }
```

The method is used so that when the questionnaire property changes its value and updates the user interface. In any view where the data might change in the view model and you want those changes to be reflected on the screen, you need to implement this pattern for all the properties in the view model.

The questionnaire property, the collection property, and the command property are initialized in the view model class constructor.

```C#
    public QuestionnaireViewModel()
    {
        this.questionnaire  new Questionnaire();
        this.AllColors  new[] { "Red", "Blue", "Green" };
    }
```

Collection properties should always be initialized to either an empty collection or, if it is appropriate, to populate the collection in the constructor, you can do so, typically by calling a service. If you do that, make sure you do so in a way that will not break the designer.

Additionally, if you expose **ICommand** properties that the view can bind command properties to, you need to create an instance of a command object. In this case, because you will use the **DelegateCommand** type from Prism, you need to create an instance of that and point it to a handling method. **DelegateCommand** also has the ability to carry along a strongly typed parameter if the **CommandParameter** property of a source control is also set. This is not used in the QuickStart, so the argument type is just specified as object.

The following code shows the **OnSubmit** handler method, located in the **MainWindowViewModel** class.

```C#
    private void OnSubmit(object obj)
    {
        Debug.WriteLine(BuildResultString());
    }
```

To keep the solution simple, this handler method returns the values entered for the questions to the Output window in Visual Studio with the help of a helper method that is already implemented in the view model class. A real implementation of a command handler would typically do something like call out to a service to save the results to a repository or retrieve data if it was a Load type of operation. It might also cause navigation to another view to occur by calling to a navigation service.

## Wiring Up the View Elements to the View Model

The bindings in the view elements point to the view model properties, as shown in the following table. Note that these bindings are located in both the MainWindow view and in the QuestionnaireView view.

Element name | Property | Value
--- | :---: | ---
NameTextBox | Text | {Binding Path Questionnaire.Name, ModeTwoWay}
AgeTextBox | Text | {Binding PathQuestionnaire.Age, ModeTwoWay}
Question1Response | Text | {Binding PathQuestionnaire.Quest, ModeTwoWay}
ColorRun | Foreground | {Binding Questionnaire.FavoriteColor, TargetNullValueBlack}
Colors | ItemsSource | {Binding PathAllColors}
Colors | SelectedItem | {Binding Questionnaire.FavoriteColor, ModeTwoWay}
SubmitButton | Command | {Binding SubmitCommand}
ResetButton | Command | {Binding ResetCommand}

## Creating the View and View Model and Hooking Them Up

There are several ways of hooking up the view model with the view. You can create the view model in the view's code behind and set it in its **DataContext** property or set it declaratively in the view's Xaml code. To instantiate the view model in XAML, the view model class must have a default constructor.

Another approach, is creating a component that can locate the corresponding view model and put it in the **DataContext** automatically, this component is typically called **View Model Locator**.

Prism provides an implementation of the View Model Locator pattern, which is an attached property.

Open MainWindow.xaml and look for the code where the view model locator property is attached. The attached property is shown in the last line of the following code.

```XAML
    <Window x:Class"BasicMVVMQuickstart_Desktop.Views.MainWindow"
            xmlns"http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x"http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:basicMvvmQuickstartDesktop"clr-namespace:BasicMVVMQuickstart_Desktop"
            xmlns:viewModel"clr-namespace:Microsoft.Practices.Prism.Mvvm;assemblyMicrosoft.Practices.Prism.Mvvm.Desktop"
            xmlns:d"http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc"http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:views"clr-namespace:BasicMVVMQuickstart_Desktop.Views"
            mc:Ignorable"d"
            Title"Basic MVVM Quickstart"
            Height"350"
            Width"525"
            d:DataContext"{d:DesignInstance basicMvvmQuickstartDesktop:QuestionnaireViewDesignViewModel, IsDesignTimeCreatableTrue}"
            viewModel:ViewModelLocator.AutoWireViewModel"True">
```

Prism's view model locator is an attached property that when set to true it will try to locate the view model of the view, and then set the view's data context to an instance of the view model. To locate the corresponding view model, the view model locator uses two approaches. First it will look for the view model in a view name/view model registration mapping. If a registration is not found, it will fall back to a convention-based approach, that will locate the view models, by replacing “.View” from the view namespace with “.ViewModel” and appending ‘’ViewModel’’ to the view’s name. For more information about ways to hook up views to view models; see "[Implementing the MVVM Pattern](/patterns-practices/guide/5-implementing-the-mvvm-pattern-using-the-prism-library-5.0-for-wpf)."

## Adding Design-Time Support

When you use the view model locator, your view models are created at runtime. Therefore, when you are designing your view, the view model is not yet constructed and you will not see the view model data at design time.

To solve this situation, you can use the **d:DataContext** designer property and set it to a view model created specifically for design time. This view model will be constructed only at design time, it is a simplification on the real view model, and may contain mock data.

You can see how this property is used in the MainWindow page, in the following code.

```XAML
    d:DataContext="{d:DesignInstance basicMvvmQuickstartDesktop:QuestionnaireViewDesignViewModel, IsDesignTimeCreatable=True}"
```

Note that you need to specify the class that will be used as the **DesignInstance**, and then set the **IsDesignTimeCreatable** property to **true**. The design view model class used as **DesignInstance** is a class that must have a default constructor.

You can see how the design view model for the QuickStart is defined, in the following code:

```C#
    public class QuestionnaireViewDesignViewModel
    {
        public QuestionnaireViewDesignViewModel()
        {
            this.QuestionnaireViewModel  new QuestionnaireViewModel();
        }

        public QuestionnaireViewModel QuestionnaireViewModel { get; set; }
    }
```

This design view model just has to initialize the properties used in the view for binding and populate them with mock data. As it is used only for design, it is not necessary to derive from **BindableBase**, nor implement the **INotifyPropertyChanged** interface.

## More Information


For more information about implementing the MVVM pattern, see the following topics:

-   [Implementing the MVVM Pattern](/patterns-practices/guide/5-implementing-the-mvvm-pattern-using-the-prism-library-5.0-for-wpf)
-   [Advanced MVVM Scenarios](/patterns-practices/guide/6-advanced-mvvm-scenarios-using-the-prism-library-5.0-for-wpf)

To learn about other code samples included with Prism, see the following topics:

-   [Stock Trader Reference Implementation](/patterns-practices/guide/stock-trader-reference-implementation-using-the-prism-library-5.0-for-wpf)
-   [Modularity QuickStarts](/patterns-practices/guide/modularity-quickstarts-using-the-prism-library-5.0-for-wpf)
-   [Interactivity QuickStart](/patterns-practices/guide/interactivity-quickstart-using-the-prism-library-5.0-for-wpf)
-   [Commanding QuickStart](/patterns-practices/guide/commanding-quickstart-using-the-prism-library-5.0-for-wpf)
-   [UI Composition QuickStart](/patterns-practices/guide/ui-composition-quickstart-using-the-prism-library-5.0-for-wpf)
-   [State-Based Navigation QuickStart](/patterns-practices/guide/state-based-navigation-quickstart-using-the-prism-library-5.0-for-wpf)
-   [View-Switching Navigation QuickStart](/patterns-practices/guide/view-switching-navigation-quickstart-using-the-prism-library-5.0-for-wpf)
-   [Event Aggregation QuickStart](/patterns-practices/guide/event-aggregation-quickstart-using-the-prism-library-5.0-for-wpf)
