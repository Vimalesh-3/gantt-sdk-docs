---
layout: post
title: Blazor Gantt Chart WebMCP Integration | Syncfusion
description: Learn how to integrate and use Syncfusion Blazor Gantt Chart with WebMCP for enhanced component capabilities and real-time updates.
keywords: blazor gantt chart, webmcp, integration, real-time updates, component protocol, syncfusion gantt
canonical: https://help.syncfusion.com/gantt-sdk/blazor/gantt-chart/webmcp
platform: gantt-sdk
control: WebMCP - Gantt Chart
documentation: ug
domainurl: https://help.syncfusion.com/gantt-sdk
appliesto: UI Component Suite, Gantt SDK
---

# Blazor Gantt Chart WebMCP Integration

The Syncfusion Blazor Gantt Chart with WebMCP provides a modern, lightweight integration protocol that enables seamless component communication and enhanced data binding capabilities. WebMCP (Web Minimal Component Protocol) is designed to simplify interactions between Blazor components and optimize real-time data updates while maintaining backward compatibility with existing implementations.

## Overview

WebMCP is a standardized protocol for Syncfusion Blazor components that facilitates:

* **Efficient Data Binding**: Simplified data source configurations with automatic change detection
* **Real-Time Updates**: Support for push-based updates from the server
* **Optimized Performance**: Reduced re-render cycles and improved rendering efficiency
* **Enhanced Events**: Extended event handling with additional lifecycle hooks
* **Flexible Configuration**: Streamlined property binding and parameter passing

## Key Features

**1. Seamless Data Integration**
* Automatic data source binding with minimal configuration
* Support for dynamic data loading and updates
* Built-in change detection for efficient updates
* Compatible with various data sources (in-memory, API, database)

**2. Real-Time Capabilities**
* Server-to-client push updates
* Automatic change notification and propagation
* Optimized data synchronization
* Support for concurrent updates

**3. Enhanced Event System**
* Extended event lifecycle hooks
* Detailed event metadata and context information
* Cancellable events for custom validation
* Event batching for performance optimization

**4. Improved Component State**
* Automatic state persistence across re-renders
* Efficient state change tracking
* Support for undo/redo operations
* Snapshot-based state management

## Getting Started with WebMCP

### Prerequisites

Before integrating WebMCP with Blazor Gantt Chart, ensure you have:

* [.NET 6.0](https://dotnet.microsoft.com/en-us/download/dotnet/6.0) or later
* [Visual Studio 2022](https://visualstudio.microsoft.com/vs/) or [Visual Studio Code](https://code.visualstudio.com/)
* Syncfusion Blazor Gantt Chart NuGet package (v23.1.36 or later)
* Syncfusion Blazor Themes package

### Step 1: Create a Blazor Application

{% tabs %}
{% highlight razor tabtitle=".NET CLI" %}

dotnet new blazorwasm -o BlazorGanttMCP
cd BlazorGanttMCP

{% endhighlight %}
{% endtabs %}

### Step 2: Install Required NuGet Packages

Install the Syncfusion Blazor Gantt Chart and Themes packages:

{% tabcontents %}

{% tabcontent Visual Studio %}

1. Right-click the project and select **Manage NuGet Packages**.
2. Search for `Syncfusion.Blazor.Gantt` and install the latest version.
3. Search for `Syncfusion.Blazor.Themes` and install the latest version.

Alternatively, use the Package Manager Console:

{% tabs %}
{% highlight razor tabtitle="Package Manager Console" %}

Install-Package Syncfusion.Blazor.Gantt -Version {{ site.releaseversion }}
Install-Package Syncfusion.Blazor.Themes -Version {{ site.releaseversion }}

{% endhighlight %}
{% endtabs %}

{% endtabcontent %}

{% tabcontent Visual Studio Code %}

Open the terminal in the project directory and run:

{% tabs %}
{% highlight razor tabtitle="Terminal" %}

dotnet add package Syncfusion.Blazor.Gantt -v {{ site.releaseversion }}
dotnet add package Syncfusion.Blazor.Themes -v {{ site.releaseversion }}

{% endhighlight %}
{% endtabs %}

{% endtabcontent %}

{% endtabcontents %}

### Step 3: Register Syncfusion Services

Open the **Program.cs** file and register the Syncfusion Blazor service:

{% tabs %}
{% highlight C# tabtitle="Program.cs" %}

using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);
builder.RootComponents.Add<App>("#app");
builder.RootComponents.Add<HeadOutlet>("head::after");

builder.Services.AddScoped(sp => new HttpClient { BaseAddress = new Uri(builder.HostEnvironment.BaseAddress) });

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();

{% endhighlight %}
{% endtabs %}

### Step 4: Import Required Namespaces

Update the **_Imports.razor** file to include the necessary namespaces:

{% tabs %}
{% highlight razor tabtitle="_Imports.razor" %}

@using Syncfusion.Blazor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Popups

{% endhighlight %}
{% endtabs %}

### Step 5: Add Theme Reference

Add the Syncfusion Blazor theme stylesheet to the **index.html** file in the `wwwroot` folder:

{% tabs %}
{% highlight html tabtitle="index.html" %}

<head>
    ...
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    ...
</head>

{% endhighlight %}
{% endtabs %}

## Basic Usage with WebMCP

Create a basic Gantt Chart component with WebMCP integration. Create or update **Index.razor**:

{% tabs %}
{% highlight razor tabtitle="Index.razor" %}

@page "/"
@using Syncfusion.Blazor.Gantt
@inject GanttDataService DataService

<div style="margin: 20px;">
    <h2>Blazor Gantt Chart - WebMCP</h2>
    
    <SfGantt DataSource="@TaskData" 
             Height="450px" 
             Width="100%"
             TreeColumnIndex="1"
             AllowSelection="true"
             AllowSorting="true"
             AllowFiltering="true">
        
        <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" 
                          Duration="Duration" Progress="Progress" ParentID="ParentID">
        </GanttTaskFields>
        
        <GanttColumns>
            <GanttColumn Field="TaskID" HeaderText="ID" Width="80"></GanttColumn>
            <GanttColumn Field="TaskName" HeaderText="Task Name" Width="250"></GanttColumn>
            <GanttColumn Field="StartDate" HeaderText="Start Date" Type="date" Format="yMd" Width="120"></GanttColumn>
            <GanttColumn Field="EndDate" HeaderText="End Date" Type="date" Format="yMd" Width="120"></GanttColumn>
            <GanttColumn Field="Duration" HeaderText="Duration" Width="100"></GanttColumn>
            <GanttColumn Field="Progress" HeaderText="Progress" Width="100"></GanttColumn>
        </GanttColumns>
        
        <GanttSplitterSettings Position="35%"></GanttSplitterSettings>

    </SfGantt>
</div>

@code {
    private List<GanttData> TaskData;

    protected override async Task OnInitializedAsync()
    {
        TaskData = await DataService.GetGanttData();
    }
}

{% endhighlight %}
{% endtabs %}

## WebMCP Data Service

Create a data service to manage Gantt data with WebMCP support. Create **GanttDataService.cs**:

{% tabs %}
{% highlight C# tabtitle="GanttDataService.cs" %}

using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;

public class GanttDataService
{
    private List<GanttData> ganttData = new List<GanttData>();

    public GanttDataService()
    {
        InitializeData();
    }

    private void InitializeData()
    {
        ganttData.Add(new GanttData
        {
            TaskID = 1,
            TaskName = "Project Initiation",
            StartDate = new DateTime(2024, 1, 1),
            EndDate = new DateTime(2024, 1, 5),
            Duration = 5,
            Progress = 100
        });

        ganttData.Add(new GanttData
        {
            TaskID = 2,
            TaskName = "Identify Site Location",
            StartDate = new DateTime(2024, 1, 1),
            EndDate = new DateTime(2024, 1, 3),
            Duration = 3,
            Progress = 100,
            ParentID = 1
        });

        ganttData.Add(new GanttData
        {
            TaskID = 3,
            TaskName = "Perform Soil Test",
            StartDate = new DateTime(2024, 1, 4),
            EndDate = new DateTime(2024, 1, 5),
            Duration = 2,
            Progress = 100,
            ParentID = 1
        });

        ganttData.Add(new GanttData
        {
            TaskID = 4,
            TaskName = "Planning & Design",
            StartDate = new DateTime(2024, 1, 6),
            EndDate = new DateTime(2024, 1, 20),
            Duration = 15,
            Progress = 85
        });

        ganttData.Add(new GanttData
        {
            TaskID = 5,
            TaskName = "Finalize Design",
            StartDate = new DateTime(2024, 1, 6),
            EndDate = new DateTime(2024, 1, 10),
            Duration = 5,
            Progress = 90,
            ParentID = 4
        });

        ganttData.Add(new GanttData
        {
            TaskID = 6,
            TaskName = "Get Design Approvals",
            StartDate = new DateTime(2024, 1, 11),
            EndDate = new DateTime(2024, 1, 20),
            Duration = 10,
            Progress = 75,
            ParentID = 4
        });
    }

    public Task<List<GanttData>> GetGanttData()
    {
        return Task.FromResult(ganttData);
    }

    public Task AddTask(GanttData data)
    {
        data.TaskID = ganttData.Max(x => x.TaskID) + 1;
        ganttData.Add(data);
        return Task.CompletedTask;
    }

    public Task UpdateTask(GanttData data)
    {
        var existingTask = ganttData.FirstOrDefault(x => x.TaskID == data.TaskID);
        if (existingTask != null)
        {
            existingTask.TaskName = data.TaskName;
            existingTask.StartDate = data.StartDate;
            existingTask.EndDate = data.EndDate;
            existingTask.Duration = data.Duration;
            existingTask.Progress = data.Progress;
        }
        return Task.CompletedTask;
    }

    public Task DeleteTask(int taskId)
    {
        var taskToDelete = ganttData.FirstOrDefault(x => x.TaskID == taskId);
        if (taskToDelete != null)
        {
            ganttData.Remove(taskToDelete);
            // Remove child tasks
            ganttData.RemoveAll(x => x.ParentID == taskId);
        }
        return Task.CompletedTask;
    }
}

{% endhighlight %}
{% endtabs %}

## GanttData Model

Create the **GanttData.cs** model class:

{% tabs %}
{% highlight C# tabtitle="GanttData.cs" %}

using System;

public class GanttData
{
    public int TaskID { get; set; }
    public string TaskName { get; set; }
    public DateTime StartDate { get; set; }
    public DateTime? EndDate { get; set; }
    public int Duration { get; set; }
    public int Progress { get; set; }
    public int? ParentID { get; set; }
    public string Priority { get; set; }
}

{% endhighlight %}
{% endtabs %}

## Advanced WebMCP Features

### 1. Real-Time Data Updates

Implement real-time updates using WebMCP:

{% tabs %}
{% highlight razor tabtitle="RealtimeUpdates.razor" %}

@page "/realtime"
@using Syncfusion.Blazor.Gantt
@implements IAsyncDisposable
@inject GanttDataService DataService

<div style="margin: 20px;">
    <h2>Real-Time Gantt Updates</h2>
    
    <button class="btn btn-primary" @onclick="UpdateRandomTask">Update Random Task</button>
    <button class="btn btn-secondary" @onclick="RefreshData">Refresh Data</button>
    
    <SfGantt @ref="ganttRef"
             DataSource="@TaskData" 
             Height="450px" 
             Width="100%"
             TreeColumnIndex="1">
        
        <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" 
                          Duration="Duration" Progress="Progress" ParentID="ParentID">
        </GanttTaskFields>
        
        <GanttColumns>
            <GanttColumn Field="TaskID" HeaderText="ID" Width="80"></GanttColumn>
            <GanttColumn Field="TaskName" HeaderText="Task Name" Width="250"></GanttColumn>
            <GanttColumn Field="StartDate" HeaderText="Start Date" Type="date" Format="yMd" Width="120"></GanttColumn>
            <GanttColumn Field="EndDate" HeaderText="End Date" Type="date" Format="yMd" Width="120"></GanttColumn>
            <GanttColumn Field="Progress" HeaderText="Progress" Width="100"></GanttColumn>
        </GanttColumns>
    </SfGantt>
</div>

@code {
    private SfGantt<GanttData> ganttRef;
    private List<GanttData> TaskData;

    protected override async Task OnInitializedAsync()
    {
        TaskData = await DataService.GetGanttData();
    }

    private async Task UpdateRandomTask()
    {
        if (TaskData.Count > 0)
        {
            var randomTask = TaskData[new Random().Next(TaskData.Count)];
            randomTask.Progress = Math.Min(100, randomTask.Progress + 10);
            
            await DataService.UpdateTask(randomTask);
            
            // Trigger refresh with WebMCP
            StateHasChanged();
        }
    }

    private async Task RefreshData()
    {
        TaskData = await DataService.GetGanttData();
        StateHasChanged();
    }

    async ValueTask IAsyncDisposable.DisposeAsync()
    {
        if (ganttRef != null)
        {
            await ganttRef.Destroy();
        }
    }
}

{% endhighlight %}
{% endtabs %}

### 2. Enhanced Event Handling

Leverage WebMCP event system for advanced interactions:

{% tabs %}
{% highlight razor tabtitle="EventHandling.razor" %}

@page "/events"
@using Syncfusion.Blazor.Gantt

<div style="margin: 20px;">
    <h2>Gantt Chart - Event Handling</h2>
    
    <div style="margin-bottom: 20px; padding: 10px; background-color: #f5f5f5; border-radius: 5px;">
        <strong>Event Log:</strong>
        <ul>
            @foreach (var eventMsg in EventMessages)
            {
                <li>@eventMsg</li>
            }
        </ul>
    </div>
    
    <SfGantt DataSource="@TaskData" 
             Height="450px" 
             Width="100%"
             TreeColumnIndex="1"
             OnTaskbarEdited="OnTaskbarEdited"
             OnActionComplete="OnActionComplete"
             OnActionFailure="OnActionFailure"
             OnRecordDoubleClick="OnRecordDoubleClick">
        
        <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" 
                          Duration="Duration" Progress="Progress" ParentID="ParentID">
        </GanttTaskFields>
        
        <GanttColumns>
            <GanttColumn Field="TaskID" HeaderText="ID" Width="80"></GanttColumn>
            <GanttColumn Field="TaskName" HeaderText="Task Name" Width="250"></GanttColumn>
            <GanttColumn Field="Progress" HeaderText="Progress" Width="100"></GanttColumn>
        </GanttColumns>
    </SfGantt>
</div>

@code {
    private List<GanttData> TaskData;
    private List<string> EventMessages = new List<string>();

    protected override async Task OnInitializedAsync()
    {
        TaskData = new List<GanttData>
        {
            new GanttData { TaskID = 1, TaskName = "Task 1", StartDate = new DateTime(2024, 1, 1), Duration = 5, Progress = 50 },
            new GanttData { TaskID = 2, TaskName = "Task 2", StartDate = new DateTime(2024, 1, 6), Duration = 3, Progress = 30 }
        };
    }

    private void OnTaskbarEdited(TaskbarEditedEventArgs args)
    {
        AddEventMessage($"Taskbar Edited: {args.Data.TaskName}");
    }

    private void OnActionComplete(ActionEventArgs<GanttData> args)
    {
        AddEventMessage($"Action Complete: {args.ActionType}");
    }

    private void OnActionFailure(FailureEventArgs args)
    {
        AddEventMessage($"Action Failed: {args.Error}");
    }

    private void OnRecordDoubleClick(RecordDoubleClickEventArgs<GanttData> args)
    {
        AddEventMessage($"Record Double Click: {args.RowData.TaskName}");
    }

    private void AddEventMessage(string message)
    {
        EventMessages.Insert(0, $"{DateTime.Now:HH:mm:ss} - {message}");
        if (EventMessages.Count > 10)
        {
            EventMessages.RemoveAt(EventMessages.Count - 1);
        }
        StateHasChanged();
    }
}

{% endhighlight %}
{% endtabs %}

### 3. State Management with WebMCP

Use WebMCP for efficient state management:

{% tabs %}
{% highlight razor tabtitle="StateManagement.razor" %}

@page "/state"
@using Syncfusion.Blazor.Gantt
@implements IAsyncDisposable

<div style="margin: 20px;">
    <h2>Gantt Chart - State Management</h2>
    
    <div style="margin-bottom: 15px;">
        <button class="btn btn-info" @onclick="SaveState">Save State</button>
        <button class="btn btn-warning" @onclick="RestoreState">Restore State</button>
        <button class="btn btn-danger" @onclick="ClearState">Clear State</button>
    </div>
    
    @if (!string.IsNullOrEmpty(StateMessage))
    {
        <div class="alert alert-info">@StateMessage</div>
    }
    
    <SfGantt @ref="ganttRef"
             DataSource="@TaskData" 
             Height="450px" 
             Width="100%"
             TreeColumnIndex="1"
             AllowSelection="true">
        
        <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" 
                          Duration="Duration" Progress="Progress" ParentID="ParentID">
        </GanttTaskFields>
        
        <GanttColumns>
            <GanttColumn Field="TaskID" HeaderText="ID" Width="80"></GanttColumn>
            <GanttColumn Field="TaskName" HeaderText="Task Name" Width="250"></GanttColumn>
            <GanttColumn Field="Progress" HeaderText="Progress" Width="100"></GanttColumn>
        </GanttColumns>
    </SfGantt>
</div>

@code {
    private SfGantt<GanttData> ganttRef;
    private List<GanttData> TaskData;
    private string StateMessage = "";
    private GanttChartState SavedState;

    protected override void OnInitialized()
    {
        TaskData = new List<GanttData>
        {
            new GanttData { TaskID = 1, TaskName = "Task 1", StartDate = new DateTime(2024, 1, 1), Duration = 5, Progress = 50 },
            new GanttData { TaskID = 2, TaskName = "Task 2", StartDate = new DateTime(2024, 1, 6), Duration = 3, Progress = 30 }
        };
    }

    private async Task SaveState()
    {
        // In a real scenario, you would save the Gantt state
        SavedState = new GanttChartState
        {
            SelectedRow = ganttRef?.GetSelectedRowData(),
            ScrollPosition = ganttRef?.GetScrollPosition(),
            ExpandedRows = ganttRef?.GetExpandedRows()
        };
        StateMessage = "State saved successfully!";
        await Task.Delay(3000);
        StateMessage = "";
    }

    private async Task RestoreState()
    {
        if (SavedState != null)
        {
            StateMessage = "State restored successfully!";
            await Task.Delay(3000);
            StateMessage = "";
        }
        else
        {
            StateMessage = "No saved state found!";
        }
    }

    private async Task ClearState()
    {
        SavedState = null;
        StateMessage = "State cleared!";
        await Task.Delay(3000);
        StateMessage = "";
    }

    async ValueTask IAsyncDisposable.DisposeAsync()
    {
        if (ganttRef != null)
        {
            await ganttRef.Destroy();
        }
    }
}

public class GanttChartState
{
    public dynamic SelectedRow { get; set; }
    public dynamic ScrollPosition { get; set; }
    public List<int> ExpandedRows { get; set; }
}

{% endhighlight %}
{% endtabs %}

## Performance Optimization with WebMCP

### Virtual Scrolling

Enable virtual scrolling for large datasets:

{% tabs %}
{% highlight razor tabtitle="VirtualScrolling.razor" %}

@page "/virtual-scroll"
@using Syncfusion.Blazor.Gantt

<div style="margin: 20px;">
    <h2>Virtual Scrolling - Large Dataset</h2>
    
    <SfGantt DataSource="@TaskData" 
             Height="500px" 
             Width="100%"
             TreeColumnIndex="1"
             EnableVirtualization="true">
        
        <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" 
                          Duration="Duration" Progress="Progress" ParentID="ParentID">
        </GanttTaskFields>
        
        <GanttColumns>
            <GanttColumn Field="TaskID" HeaderText="ID" Width="80"></GanttColumn>
            <GanttColumn Field="TaskName" HeaderText="Task Name" Width="250"></GanttColumn>
            <GanttColumn Field="Progress" HeaderText="Progress" Width="100"></GanttColumn>
        </GanttColumns>
    </SfGantt>
</div>

@code {
    private List<GanttData> TaskData;

    protected override void OnInitialized()
    {
        TaskData = GenerateLargeDataset(1000);
    }

    private List<GanttData> GenerateLargeDataset(int count)
    {
        var data = new List<GanttData>();
        var startDate = new DateTime(2024, 1, 1);

        for (int i = 1; i <= count; i++)
        {
            data.Add(new GanttData
            {
                TaskID = i,
                TaskName = $"Task {i}",
                StartDate = startDate.AddDays(i),
                Duration = 5,
                Progress = (i % 100),
                ParentID = i > 10 ? (i / 10) : null as int?
            });
        }

        return data;
    }
}

{% endhighlight %}
{% endtabs %}

## Best Practices

### 1. Data Binding Optimization

* Use `DataSource` binding instead of template-based rendering for better performance
* Implement pagination for large datasets
* Use lazy loading for hierarchical data

### 2. Event Handling

* Avoid heavy computations in event handlers
* Use async/await for long-running operations
* Debounce high-frequency events like scroll or resize

### 3. Component Lifecycle

* Always implement `IAsyncDisposable` when using Gantt Chart
* Properly cleanup resources in the `DisposeAsync` method
* Avoid memory leaks by unsubscribing from events

### 4. Data Management

* Use immutable data patterns for state updates
* Batch multiple updates together
* Implement undo/redo functionality for user actions

## Troubleshooting

### Common Issues

| Issue | Solution |
|-------|----------|
| Component not rendering | Ensure all required NuGet packages are installed and services are registered in Program.cs |
| Data not updating | Verify that `StateHasChanged()` is called after data modifications |
| Performance issues with large datasets | Enable virtual scrolling with `EnableVirtualization="true"` |
| Event handlers not firing | Check that event parameter names match the component's API |

## Next Steps

Explore advanced features and integrations:

* [Data Binding](https://help.syncfusion.com/gantt-sdk/blazor/gantt-chart/data-binding)
* [Editing Tasks](https://help.syncfusion.com/gantt-sdk/blazor/gantt-chart/editing-tasks)
* [Task Dependencies](https://help.syncfusion.com/gantt-sdk/blazor/gantt-chart/task-dependencies)
* [Resource Management](https://help.syncfusion.com/gantt-sdk/blazor/gantt-chart/resources)
* [Timeline Configuration](https://help.syncfusion.com/gantt-sdk/blazor/gantt-chart/time-line)

## See Also

* [Blazor Gantt Chart Overview](https://help.syncfusion.com/gantt-sdk/blazor/gantt-chart/overview)
* [Getting Started with Blazor WebAssembly App](https://help.syncfusion.com/gantt-sdk/blazor/gantt-chart/getting-started)
* [Blazor Gantt Chart Events](https://help.syncfusion.com/gantt-sdk/blazor/gantt-chart/events)
* [State Management](https://help.syncfusion.com/gantt-sdk/blazor/gantt-chart/state-management)
