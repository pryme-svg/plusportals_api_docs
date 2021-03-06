# Unofficial PlusPortals API Documentation


## Endpoints

Root URL: `https://plusportals.com`

### `/ParentStudentDetails/GetMarkingPeriod`

Description: Get marking periods (semesters/trimesters/quarters)

Method: `GET`

Headers Required: `(none)`

Cookies Required: 

```
{
	"Request Cookies": {
		"__RequestVerificationToken": "",
		"_pps": "-480",
		".ASPXAUTH": "",
		"ASP.NET_SessionId": "",
		"emailoption": "",
		"ppschoollink": "",
		"UGUID": "",
		"UserSessionId": ""
	}
}
```

Response: `json`

```
[
  {
    "MarkingPeriodId": 0,
    "MarkingPeriodName": "Please select...",
    "Description": "Please select...",
    "IsChecked": false,
    "StudentId": {},
    "Narrative": null,
    "isCurrentmarkingPeriod": false,
    "Token": null,
    "AverageColumn": false,
    "GradeColumn": false,
    "ReportAndScorePermission": false,
    "CustomMessage": null,
    "IsEmail": false,
    "ShowClassesWithBlankAndSpaces": false
  },
  {
    "MarkingPeriodId": {},
    "MarkingPeriodName": "{} ",
    "Description": "Semester {}",
    "IsChecked": false,
    "StudentId": {},
    "Narrative": null,
    "isCurrentmarkingPeriod": false,
    "Token": null,
    "AverageColumn": false,
    "GradeColumn": false,
    "ReportAndScorePermission": false,
    "CustomMessage": null,
    "IsEmail": false,
    "ShowClassesWithBlankAndSpaces": false
  },
  ...
]
```

### `/ShowMarkingPeriodByType`

Description: Get marking periods but omit "Please Select"

Method: `GET`

Response: `json`

### `/SCHOOL_NAME`

Description: Login

Method: `POST`

Payload:

```
payload = {
    'UserName': email,
    'Password': password,
    'RememberMe': 'true',
    'btnsumit': 'Sign In'
}
```

Response: cookies

```
"Response Cookies": {
	"emailoption": {
		"expires": "2021-12-26T01:56:56.000Z",
		"httpOnly": true,
		"path": "/",
		"value": "RecentEmails"
	},
	"ppschoollink": {
		"expires": "2021-12-26T01:56:56.000Z",
		"httpOnly": true,
		"path": "/",
		"value": "SCHOOL_NAME"
	},
	"UserSessionId": {
		"expires": "2021-12-24T01:56:56.000Z",
		"path": "/",
		"value": ""
	}
}
```

### `/ParentStudentDetails/ShowGridProgressInfo`

Description: Get Grades

Method: `POST`

Cookies Required:

```
_pps: '-480'
ppschoollink
UGUID
__RequestVerificationToken (from cookies)
ASP.NET_SessionID
emailoption
.ASPXAUTH
UserSessionId
```

All of the cookies above can be retrieved from `Login` except `_pps`

Headers Required:

```
'X-NewRelic-ID': 'XQEDUV5SGwUDXFhXBQc=',
'__RequestVerificationToken': {},
'X-Requested-With': 'XMLHttpRequest'
```

> IMPORTANT: RequestVerificationToken in headers is retrieved from the html, not the cookies. It is of the form `<input name="__RequestVerificationToken" type="hidden" value={}>`

Params (optional):

```
'markingPeriodId': {},
'isGroup': true/false
```

Response: `json`

```json
{
	"Data": [{
		"CourseId": 0, # this is always 0, probably bug
		"CourseName": "",
		"Average": int,
		"GradeSymbol": "A  ",
		"StaffName": null,
		"MarkingPeriodId": 0,
		"StudentId": int,
		"SectionId": int,
		"IsWithdrawnCourse": false,
		"StaffEmailId": null,
		"ShowClassesWithBlankAndSpaces": false
	}, {
		...
	}],
	"Total": int,
	"AggregateResults": null,
	"Errors": null
}
```

### `/ParentStudentDetails/GetCourseworkDetailsByStudent`

Description: Get Coursework

Method: `POST`

Cookies Required: 

```
"__RequestVerificationToken": "" (from cookies),
"_pps": "-480",
".ASPXAUTH": "",
"ASP.NET_SessionId": "",
"emailoption": "",
"ppschoollink": "",
"UGUID": "",
"UserSessionId": ""
```

Payload:

```json
{
	"sort": "",
	"page": "1",
	"pageSize": "20",
	"group": "",
	"filter": "",
	"studentId": "{}",
	"sectionId": "0",
	"courseworkRange": "1-5",
	"courseworkType": "All+Types"
}
```

courseworkRange:

```
"Upcoming Coursework": 1,
"Past Due Coursework": 2,
"Pending Coursework": 3,
"Completed Coursework": 4,
"All Coursework": 5
```

sort:

```
"Class-asc"
"Class-desc"
"Title-asc"
"Title-desc"
"Description-asc"
"Description-desc"
"Type-asc"
"Type-desc"
"DueDate-asc"
"DueDate-desc"
```

courseworkType:

```
All Types: "All+Types"
Assignments: "Assignments",
Quizzes: "Quizzes"
```

sectionId is the ID of the class which you would like to see. Can be retrieved from [Grades](#parentstudentdetailsshowgridprogressinfo).


### `/ParentStudentDetails/ShowScoresGridInfo`

Description: Get grades for all assignments

Method: `POST`

Response: `json`

> !! Note: the class that grades are shown for is affected by the last `/ParentStudentDetails/ShowScoresDetails` request

### `/ParentStudentDetails/GetPeriodAttendanceBySpec`

Method: `POST`

Response: `json`

```
{
  "Data": [
    {
      "AttendanceCount": int,
      "AttendanceDescription": "ABSENCE EX",
      "AttendanceDate": "/Date(-int)/"
    },
    {
	...
    }
  ],
  "Total": int,
  "AggregateResults": null,
  "Errors": null
}
```

### `/ParentStudentDetails/ShowAttendanceGridInfo`

Description: Daily Attendance tab

Method: `POST`

Response: `json`

### `/ParentStudentDetails/GetPeriodAttendance/`

Description: Period Attendance Tab

Method: `GET`

Response: `html`

### `/ELocker/GetSectionsforStudent`

Method: `POST`

Response: `json`

Params:

```

studentId: {}
```

Response: `json`

```
[
  {
    "Disabled": false,
    "Group": null,
    "Selected": false,
    "Text": "All Classes",
    "Value": "0"
  },
  {
    "Disabled": false,
    "Group": null,
    "Selected": false,
    "Text": "",
    "Value": ""
  },
  {
	  ...
  }
]
```

### `/ParentStudentDetails/GetSchoolLinksNFiles`

Method: `POST`

Payload: 

```
{
	"sort": "",
	"group": "",
	"filter": "",
	"labelTagId": "",
	"gradeLevel": "int"
}
```

Response:

```json
{
  "Data": [
    {
      "IsLink": true,
      "LinkText": "",
      "LinkURL": "",
      "DateAdded": "/Date(-62135596800000)/",
      "SchoolID": 0,
      "YearID": 0,
      "IsDeleted": false,
      "ModifiedBy": 0,
      "LastModifiedOn": "/Date(-62135596800000)/",
      "ID": int,
      "SectionId": null,
      "SectionFileName": null,
      "Description": "",
      "Comments": null,
      "StorageFileName": null,
      "StorageFilePath": null,
      "IsArchived": false,
      "resourceType": null
    },
    {
    	...
    }
  ],
  "Total": int,
  "AggregateResults": null,
  "Errors": null
}
```

### `/Admin/ShowFolderForSchoolResource`

Description: Get school folders

Method: `GET`

Response: `json`

```
[
	{
		"LabelTagID": 0,
		"LabelTagDescription": "School Folder",
		"LabelTagType": null,
		"LastModifiedBy": 0,
		"SectionID": null,
		"CustomGroupID": null,
		"isGroup": false,
		"UnitID": null,
		"IsSchoolResource": false
	},
	{
		...
	},
```

### `/ParentStudentDetails/LoadSectionDropdown`

Description: Get Course info

Method: `GET`

Params: 

```
"isGroup": "false"
```

Response: `json`

```
[
	{
		"CourseId": int,
		"Name": "",
		"Teacher": "",
		"Room": "69420",
		"MeetingTime": "F",
		"ClassLength": "All Year",
		"SectionType": "N"
	},
	{
		...
	}
]
```

### `/ParentStudentDetails/GetCalendarEventsStudent`

Method: `POST`

Params: Get calendar events

```
isGroup: "False"
```

Payload:

```
{
	"sort": "",
	"group": "",
	"filter": ""
}
```

Response: `json`

```
{
	"Data": [
		{
			"MessageId": int,
			"Title": null,
			"Description": "",
			"Date": "12-31-2021",
			"EndDate": "12-31-2021",
			"IsAlert": false,
			"NotificationFilesCount": 0,
			"NotificationFiles": null,
			"GoogleCalendarId": null,
			"GoogleCalendarName": null,
			"EventIsReccurrence": false,
			"IsAllDayEvent": true,
			"ParsedRecurrenceStartDate": "12-31-2021",
			"ParsedRecurrenceEndDate": "12-31-2021",
			"EventStartDate": "/Date(1640563200000)/",
			"CalendarType": null
		},
		{
			...
		},
	],
	"Total": int,
	"AggregateResults": null,
	"Errors": null
}
```

---

In Class stuff

### `/ParentStudentDetails/ShowClassDetails`

Params: 

```
sectionId: "",
groupId: "0"
```

Response: `html`

### `/TeacherClassDetails/GetSectionClassFiles`

Method: `POST`

The response of this depends on the last `ShowClassDetails` call. The last accessed with `ShowClassDetails` will have their files returned.
