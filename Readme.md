# How to conditionally show and hide Time Regions in Scheduler Control

Scheduler Control allows you to highlight specific time frames: [Time Regions](https://docs.devexpress.com/WPF/401378/controls-and-libraries/scheduler/time-regions). Using this approach, you can highlight launch time, a holiday, a certain day of week, and etc.

When you use this approach, you can notice that MonthView displays all Time Regions defined for a certain day like these regions occupy the whole day. This example illustrates how you can hide Time Regions that should not be shown in MonthView.

The main idea of this approach is to handle SchedulerControl's **FilterTimeRegion** event and set the **e.Visible** to **False** if a certain Time Region should not be shown. In this example, Scheduler contains two Time Regions: 

```xaml
<dxsch:SchedulerControl.TimeRegionItems>
    <dxsch:TimeRegionItem BrushName="{x:Static dxsch:DefaultBrushNames.TimeRegion3Hatch}"
                            Start="1/1/2019 13:00:00"
                            End="1/1/2019 14:00:00"
                            RecurrenceInfo="{dxsch:RecurrenceDaily Start='1/1/2019 13:00:00', ByDay=WorkDays}"
                            Type ="Pattern" />
    <dxsch:TimeRegionItem BrushName="{x:Static dxsch:DefaultBrushNames.TimeRegion1Dotted}"
                            Start="1/1/2019 00:00:00"
                            End="1/1/2019 23:59:59"
                            RecurrenceInfo="{dxsch:RecurrenceWeekly Start='1/1/2019 00:00:00', ByDay='Wednesday'}"
                            Type ="Pattern" />
</dxsch:SchedulerControl.TimeRegionItems>
```

The first Time Region highlights the launch time and should not be visible when MonthView becomes active. To hide this region in MonthView, we set the **e.Visible** property **False** in the **FilterTimeRegion** event handler when the **e.View** is **MonthView** and the Time Region's interval less than 23 hours. We used our **DXEvent** to process the FilterTimeRegion event in XAML: 

```xaml
<dxsch:SchedulerControl ...
       FilterTimeRegion="{DXEvent Handler='@args.Visible = @args.TimeRegion.Interval.Duration.TotalHours > 23 or !(@args.View is $dxsch:MonthView)'}">
    ...
</dxsch:SchedulerControl>
```

For more information, please refer to these help topics: 
[DXEvent](https://docs.devexpress.com/WPF/115778/mvvm-framework/dxbinding/dxevent)
[Language Specification](https://docs.devexpress.com/WPF/115777/mvvm-framework/dxbinding/language-specification)