# How to show a custom agenda view in the Flutter event calendar (SfCalendar) widget?

In the flutter event calendar, you can integrate custom agenda view using the appointment details from the `OnTap` callback of the calendar. 

## Step 1:
In initState(), set the default value for the calendar view.

```xml
CalendarView _calendarView;
List<Appointment> appointmentDetails;
 
@override
void initState() {
  appointmentDetails = <Appointment>[];
  _calendarView = CalendarView.month;
  super.initState();
}
```
 
## Step 2:
Trigger the `OnTap` callback of the calendar widget and get the appointment details from the `targetElement`. Using the appointment details, you can provide data to the custom agenda view below the calendar widget as per your requirement.

```xml
Expanded(
  child: SfCalendar(
    view: _calendarView,
    dataSource: getCalendarDataSource(),
    onTap: calendarTapped,
  ),
),
void calendarTapped(CalendarTapDetails calendarTapDetails) {
  if (calendarTapDetails.targetElement == CalendarElement.calendarCell) { 
    setState(() {
      appointmentDetails = calendarTapDetails.appointments;
    });
  }
}
```

## Step 3:
Use the ` ListView` widget inside the `Expanded` widget for showing appointment details. You can also use the `ListTile` widget to customize the agenda view with appointment details.

```xml
body: Column(
  children: <Widget>[
    Expanded(
      child: SfCalendar(
        view: _calendarView,
        dataSource: getCalendarDataSource(),
        onTap: calendarTapped,
      ),
    ),
    Expanded(
        child: Container(
            color: Colors.black12,
            child: ListView.separated(
              padding: const EdgeInsets.all(2),
              itemCount: appointmentDetails.length,
              itemBuilder: (BuildContext context, int index) {
                return Container(
                    padding: EdgeInsets.all(2),
                    height: 60,
                    color: appointmentDetails[index].color,
                    child: ListTile(
                      leading: Column(
                        children: <Widget>[
                          Text(
                            appointmentDetails[index].isAllDay
                                ? ''
                                : '${DateFormat('hh:mm a').format(appointmentDetails[index].startTime)}',
                            textAlign: TextAlign.center,
                            style: TextStyle(
                                fontWeight: FontWeight.w600,
                                color: Colors.white,
                                height: 1.7),
                          ),
                          Text(
                            appointmentDetails[index].isAllDay
                                ? 'All day'
                                : '',
                            style: TextStyle(
                                height: 0.5, color: Colors.white),
                          ),
                          Text(
                            appointmentDetails[index].isAllDay
                                ? ''
                                : '${DateFormat('hh:mm a').format(appointmentDetails[index].endTime)}',
                            textAlign: TextAlign.center,
                            style: TextStyle(
                                fontWeight: FontWeight.w600,
                                color: Colors.white),
                          ),
                        ],
                      ),
                      trailing: Container(
                          child: Icon(
                        getIcon(appointmentDetails[index].subject),
                        size: 30,
                        color: Colors.white,
                      )),
                      title: Container(
                          child: Text(
                              '${appointmentDetails[index].subject}',
                              textAlign: TextAlign.center,
                              style: TextStyle(
                                  fontWeight: FontWeight.w600,
                                  color: Colors.white))),
                    ));
              },
              separatorBuilder: (BuildContext context, int index) =>
                  const Divider(
                height: 5,
              ),
            )))
  ],
),
```
