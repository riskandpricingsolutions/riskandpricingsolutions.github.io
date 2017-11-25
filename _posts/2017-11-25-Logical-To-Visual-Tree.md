---
layout: post
title: Logical To Visual Tree
---

# Controls, logical trees and visual trees
In WPF a control's visual appearance is completely separated from its core functionality. The Control base class provides a dependency property called **Template** whose type is **ControlTemplate**. Whenever a Control is instantiated its ControlTemplate is used to generate a tree of visuals that will be used in rendering. The template acts as the blueprint that tells WPF how to create the visual elements needed to render the Control. Typically a WPF interface’s visual tree is an expansion of its logical tree with each node in the logical tree mapping to a sub-tree of elements in the visual tree.  In the following sections I will show how simple logical trees are expanded into visual trees via template instantiation.


## Window with empty ControlTemplate
The following piece of code sets up a logical tree of a single Window with an empty ControlTemplate. 

```xml
	<Window x:Class="Controls_05.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d"
        Title="MainWindow" Height="350" Width="525">
    <Window.Template>
        <ControlTemplate />
    </Window.Template>
</Window>
```

In this reductionist example the logical tree and the visual tree are the same. 


![](/assets/2017-11-25-Logical-To-Visual-Tree/A2E29814-7E77-4E7E-902D-29675AC7CF8B.png)



## Window with simplistic ControlTemplate
We can replace the empty ControlTemplate from the previous section with a simplistic visual tree as follows

```xml
<Window x:Class="Controls_001.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d"
        Title="MainWindow" Height="350" Width="525">
    <Window.Template>
        <ControlTemplate>
            <StackPanel Orientation="Horizontal" Background="LightBlue">
                <Rectangle Width="50" Height="50" Fill="Red" Margin="10"/>
                <Rectangle Width="50" Height="50" Fill="Blue" Margin="10"/>
            </StackPanel>
        </ControlTemplate>
    </Window.Template>
</Window>
```

Now the logical and visual tree are no longer the same.  The single node logical tree expands to a four node visual tree

![](/assets/2017-11-25-Logical-To-Visual-Tree/E13FC017-983B-4D1E-95BF-2569373E9DC0.png)

## Rendering Content
A Window is a ContentControl which mean is should render a single piece of content. The manner in which implemented the ControlTemplate in the previous example meant that window would always render the same irrespective of the content.  We created a ControlTemplate that ignored Content. This is clearly undesirable. We now consider how to create a ControlTemplate for our window that correctly renders content. 

The most common way to render Content in a ContentControl is via an instance of the ContentPresenter class. ContentPresenter is a FrameworkElement designed for rendering heterogenous content. Every WPF ContentControl uses an instance of ContentPresenter in its default ControlTemplate. The ContentPresenter has three core DependencyProperties.

* Content
* ContentTemplate
* ContentTemplateSelector

Whenever a ContentPresenter is inside the ControlTemplate of a ContentControl these three properties automatically take there values from the properties of the same name in the parent ContentControl

![](/assets/2017-11-25-Logical-To-Visual-Tree/C0C2758C-B7C9-4977-818E-E2177C6775C2.png)

Let us add a ContentPresenter now to our Window’s ControlTemplate and see how it renders the window. By adding a piece of content to our Window we create a two node logical tree which we can expands into a four node visual tree as follows

![](/assets/2017-11-25-Logical-To-Visual-Tree/C7CC0EE8-28A4-487D-93F0-731848D84676.png)

How does the ContentPresenter know to convert the Windows content string to a TextBlock and set its TextProperty to be the content string? This is one of the rules of the WPF content model which we can summarise as follows. 

1. If the content is a UI Element render it as is
2.  If the content is a string render it in a TextBlock
3.  .NET type which has a data template associated with it then use the data template to render it. 
4. Otherwise render the standard .NET objects class name in a TextBlock

The next section will show how to provide a more complex piece of content together using a control template for the window and a data template to render a complex piece of context

## Control and Data Templates combine
In this example the window’s content is a complex object which we render via a DataTemplate. 

```xml
<Window x:Class="Controls_003.MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:controls003="clr-namespace:Controls_003"
        mc:Ignorable="d"
        Title="MainWindow" Height="350" Width="525">

    <Window.Resources>
        <DataTemplate DataType="{x:Type controls003:Person}">
            <StackPanel Orientation="Vertical">
                <TextBlock Text="{Binding FirstName}"></TextBlock>
                <TextBlock Text="{Binding SecondName}"></TextBlock>
            </StackPanel>
        </DataTemplate>
    </Window.Resources>

    <Window.Template>
        <ControlTemplate TargetType="{x:Type controls003:MainWindow}">
            <Border Background="LightBlue">
                <ContentPresenter></ContentPresenter>
            </Border>
        </ControlTemplate>
    </Window.Template>
    <controls003:Person FirstName="Kenny" SecondName="Wilson"/>
    
</Window>
```


![](/assets/2017-11-25-Logical-To-Visual-Tree/31334367-8571-41EC-B25C-A473702FF2A4.png)


## ContentControl’s ContentTemplate
We have already seen that each control has a ControlTemplate that defines the visual tree of that Control. In addition every ContentControl also has a ContentTemplate which defines how its single piece of content is rendered.  While the Control.Template dependency property is of type ControlTemplate the ContentTemplate is of type DataTemplate.

DataTemplates and ControlTemplates while similar (both extend the same base class) are intended for different purposes. A control template defines the look of the whole control and tends to be somewhat complex. A data template defines the look of a piece of content within a content control and tends to be simpler. For instance consider the scenario where I want to render a non UIElement object as the content of a button. 

The following example is a little contrived but it uses two control templates (one for window and one for button), a content template (for button) and a data template to render the person object. In reality one might use a content template to provide some simpler customisation rather than writing the 	whole control template from scratch)

![](/assets/2017-11-25-Logical-To-Visual-Tree/6200A90B-59BA-4CEC-B5EA-ECB1F677AD28.png)

## Dependency Properties and Logical/Visual Tree

![](/assets/2017-11-25-Logical-To-Visual-Tree/C841A755-1ACA-4BAD-8D40-B1E87BA29B6C.png)

1. The window sets its DataContent to an instance of type Person, a non visual element. As DataContext is an inheritable DependencyProperty it is inherited by the windows decedents in the visual tree. 
2. The elements generated by the Window’s Template can reference their containing Control. This reference is known as the TemplatedParent
3. A ContentPresenter instantiated as part of a Control’s ControlTemplate implicitly uses the TemplatedParents Content property as its own Content.
4. The windows Content property value is set to a Binding. The binding only sets the Path property thereby implicitly using the DataSource as the Binding source
5. The TextBlock is generated from a template that is used to render strings. As such its TemplatedParent is the ContentPresenter 


## ControlTemplate Triggers
A Template supports the same complete set of triggers as a style. 

## Visual State
A control specifies a set of visual states which a template must handle 




#csharp/wpf/controls

