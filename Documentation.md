
# SmartThings Unofficial Documentation

Many of the endpoints (and parameters for them) documented below were discovered via trial and error methods. However, a more complete list of endpoints can be found in this [Titanium project here.](https://github.com/alanleard/ti.smart/blob/master/src/lib/smart.js) However, it should be noted that not all of those seem to work. The endpoints documented here have been tested. No promises things won't change and it should be expected that they will.

## Projects
You first need an account on [http://build.smartthings.com](http://build.smartthings.com) which is different than your account for using your device (which is used with http://graph.api.smarthings.com). Then you can go to create a project at the following URL: [http://build.smartthings.com/projects/create/](http://build.smartthings.com/projects/create/)

*Tip: When creating a project, you will have the option to keep it public or private. If you're just playing around, it may be a good idea to not open it up to being discovered and the used by others.*

More information here: [http://build.smartthings.com/smartthings-projects/](http://build.smartthings.com/smartthings-projects/)

### Project Pages
Your project will have its own little homepage on the SmartThings site. You're given a project blog, galleries, and others users can comment, etc. SmartThings wants to have its own social network for developers and projects and they seem to be using WordPress to get the job done.

## Project Settings
You can configure a few settings for your project. In the top left you'll see a "My Projects" menu item and from there you can access one of your projects. Go to "Dashboard" from that menu. Then on the left menu you'll see "Project Settings" and in here you can change your project name, description, image, etc. This is where you would also change the privacy setting so that you can change the app from private to public.

## JSON API Endpoints
First off, you need to be logged into http://graph.api.smarthings.com … Which is simple enough. You can then go on to see a list of your hubs and events, among other things, in their dashboard. However, we're after JSON responses and all of hte data you see here is available in (seemingly open) JSON responses.

For the purposes of the following example responses, note that  `<idHashString>` values represent 32 character long alpha-numeric unique IDs. You will see other placeholder values as well, but the `<idHashString>` values will be useful to make other API calls. You will see another hash string placeholder value `<uuid>` which is a normal [UUID](http://en.wikipedia.org/wiki/Universally_unique_identifier). All other placeholder values are simply strings if not clearly specified.

### Authenticating
Before you can make any of these HTTP requests you will need to be logged in. You can do that manually as explained above, but you can also with code using oauth. For additional information, see the official (though incomplete) documentation: [https://docs.google.com/document/d/14F5CnjIbI8Ru4-MrjHXhnBGGQngUevVEe7H3cqoGHKA/edit?pli=1](https://docs.google.com/document/d/14F5CnjIbI8Ru4-MrjHXhnBGGQngUevVEe7H3cqoGHKA/edit?pli=1)

Remember, every call that you will be making will display data relative to the account you are logged in with. You will not have permission to view certain data and you certainly can't get information about other accounts. 

### Accounts
Get information about your accounts.    
[https://graph.api.smartthings.com/api/accounts](https://graph.api.smartthings.com/api/accounts)

Example response:

	[
		{
			id: "<idHashString>",
			name: "<email>'s Account",
			fullName: "<yourName>",
			locked: false,
			permissions: "a"
		}
	]

### Account Details
You'll want to replace "idHashString" here in the URL with your account ID string found in the **Accounts** endpoint or **Locations** endpoint. This call will return a combination of some of the data found in some of the other endpoints. It will include your account, locations, and recent events.    
[https://graph.api.smartthings.com/api/accounts/idHashString](https://graph.api.smartthings.com/api/accounts/idHashString)

### Account Locations
Get information about locations for a specific account. Replace "idHashString" with your account ID string found in the **Accounts** endpoint or **Locations** endpoint.    
[https://graph.api.smartthings.com/api/accounts/idHashString/locations](https://graph.api.smartthings.com/api/accounts/idHashString/locations)

### Account Events
Get the recent events for an account. Replace "idHashString" with your account ID string found in the **Accounts** endpoint or **Locations** endpoint.    
[https://graph.api.smartthings.com/api/accounts/idHashString/events](https://graph.api.smartthings.com/api/accounts/idHashString/events)

**Params**    
`all=<boolean>`    
`max=<number>`    
`source=<boolean>`

The default is to display the ten most recent displayed events. Changing this will number will return more or less events. For example: https://graph.api.smartthings.com/api/accounts/idHashString/events?max=20

If `all` is true, then all events will be returned. If `source` is true, then only events not displayed back to the user will be returned. This would include some more detailed information from the sensors including if they are battery powered or not, etc.

Example response:

	[
		{
			id: "<uuid>",
			hubId: "<idHashString>",
			description: "SmartSense Motion detected motion has stopped",
			displayed: true,
			linkText: "SmartSense Motion",
			date: "2013-05-22T23:20:04.126Z",
			unixTime: 1369264804126,
			value: "inactive",
			deviceId: "<idHashString>",
			name: "motion",
			locationId: "<idHashString>"
		},
		{
			id: "<uuid>",
			hubId: "<idHashString>",
			description: "SmartSense Motion detected motion",
			displayed: true,
			linkText: "SmartSense Motion",
			date: "2013-05-22T23:19:02.291Z",
			unixTime: 1369264742291,
			value: "active",
			deviceId: "<idHashString>",
			name: "motion",
			locationId: "<idHashString>"
		},
		...
	]

### Hubs
Get information about your connected hubs.    
[https://graph.api.smartthings.com/api/hubs](https://graph.api.smartthings.com/api/hubs)

Example response:

	[
		{
			id: "<idHashString>",
			name: "My House",
			locationId: "<idHashString>",
			firmwareVersion: "000.009.00004",
			zigbeeId: "<idString>",
			status: "ACTIVE",
			onlineSince: "2013-05-22T22:00:44.858Z",
			signalStrength: null,
			batteryLevel: null,
			type: {
				name: "Hub"
			},
			virtual: false,
			permissions: "a"
		}
	]

### Hub Details
Get information about your hub, the connected devices, and recent events. The ten latest events appear to be displayed.    
[https://graph.api.smartthings.com/api/hubs/idHashString](https://graph.api.smartthings.com/api/hubs/idHashString)

Example response:

	{
		account: {
			id: "<idHashString>",
			name: "<email>'s Account",
			fullName: "<yourName>",
			locked: false,
			permissions: "a"
		},
		locations: [
			{
				id: "<idHashString>",
				name: "Old apt",
				accountId: "<idHashString>",
				latitude: "",
				longitude: "",
				regionRadius: 50,
				backgroundImage: "https://smartthings-location-images.s3.amazonaws.com/standard/standard51.jpg",
				mode: {
					id: "<idHashString>",
					name: "Home"
				},
				modes: [
					{
						id: "<idHashString>",
						name: "Away"
					},
					{
						id: "<idHashString>",
						name: "Home"
					},
					{
						id: "<idHashString>",
						name: "Night"
					}
				],
				permissions: "a"
			}
		],
		events: [
			{
				id: "<uuid>",
				hubId: "<idHashString>",
				description: "SmartSense Motion detected motion has stopped",
				displayed: true,
				linkText: "SmartSense Motion",
				date: "2013-05-23T00:11:10.900Z",
				unixTime: 1369267870900,
				value: "inactive",
				deviceId: "<idHashString>",
				name: "motion",
				locationId: "<idHashString>"
			},
			…
		]
	}

### Hub Events
Get the last few (currently 10) events for a hub.    
[https://graph.api.smartthings.com/api/hubs/idHashString/events](https://graph.api.smartthings.com/api/hubs/idHashString/events)

**Params**    
`all=<boolean>`    
`max=<number>`    
`source=<boolean>`

*Note: The response looks just like the account events endpoint response.*

### Hub Devices
Get the devices paired with a hub (also available in the hub details response). This is a great endpoint to access in order to get the current states of devices; for example, the current temperature from a multi sensor.   
[https://graph.api.smartthings.com/api/hubs/idHashString/devices](https://graph.api.smartthings.com/api/hubs/idHashString/devices)

Example response:

	[
		{
			id: "<idHashString>",
			name: "SmartSense Motion",
			hubId: "<idHashString>",
			label: null,
			status: "ACTIVE",
			currentStates: [
				{
					name: "motion",
					value: "inactive",
					unit: null,
					date: "2013-05-23T05:57:41Z",
					unixTime: 1369288661864
				}
			],
			typeId: "<uuid>",
			deviceNetworkId: "<id>",
			virtual: false,
			primaryTileName: null,
			stateOverrides: [ ],
			permissions: "a"
		},
		{
			id: "<idHashString>",
			name: "SmartSense Multi",
			hubId: "<idHashString>",
			label: null,
			status: "ACTIVE",
			currentStates: [
			{
				name: "threeAxis",
				value: "46,12,1016",
				unit: null,
				date: "2013-05-23T05:49:47Z",
				unixTime: 1369288187171
			},
			{
				name: "temperature",
				value: "65",
				unit: "F",
				date: "2013-05-23T05:54:50Z",
				unixTime: 1369288490264
			},
			{
				name: "acceleration",
				value: "inactive",
				unit: null,
				date: "2013-05-23T05:54:50Z",
				unixTime: 1369288490264
			},
			{
				name: "contact",
				value: "closed",
				unit: null,
				date: "2013-05-23T05:54:50Z",
				unixTime: 1369288490264
			}
		],
		typeId: "<uuid>",
		deviceNetworkId: "<id>",
		virtual: false,
		primaryTileName: "contact",
		stateOverrides: [ ],
		permissions: "a"
		}
	]

### Device Details
Get details about a specific device (given its deviceId) along with its recent events.   
[https://graph.api.smartthings.com/api/devices/idHashString](https://graph.api.smartthings.com/api/devices/idHashString)

Example response:

	{
		device: {
			id: "<idHashString>",
			name: "SmartSense Multi",
			hubId: "<idHashString>",
			label: null,
			status: "ACTIVE",
			currentStates: [
				{
					name: "temperature",
					value: "64",
					unit: "F",
					date: "2013-05-23T06:04:50Z",
					unixTime: 1369289090908
				},
				{
					name: "threeAxis",
					value: "46,12,1016",
					unit: null,
					date: "2013-05-23T05:49:47Z",
					unixTime: 1369288187171
				},
				{
					name: "acceleration",
					value: "inactive",
					unit: null,
					date: "2013-05-23T06:04:50Z",
					unixTime: 1369289090908
				},
				{
					name: "contact",
					value: "closed",
					unit: null,
					date: "2013-05-23T06:04:50Z",
					unixTime: 1369289090908
				}
			],
			typeId: "<uuid>",
			deviceNetworkId: "<id>",
			virtual: false,
			primaryTileName: "contact",
			stateOverrides: [ ],
			permissions: "a"
		},
		events: [
		{
			id: "<uuid>",
			hubId: "<idHashString>",
			description: "SmartSense Multi was 64°F",
			displayed: true,
			linkText: "SmartSense Multi",
			date: "2013-05-23T05:59:50.721Z",
			unixTime: 1369288790721,
			value: "64",
			deviceId: "<idHashString>",
			unit: "F",
			name: "temperature",
			locationId: "<idHashString>"
		},
		{
			id: "<uuid>",
			hubId: "<idHashString>",
			description: "SmartSense Multi was active",
			displayed: true,
			linkText: "SmartSense Multi",
			date: "2013-05-23T05:49:17.822Z",
			unixTime: 1369288157822,
			value: "active",
			deviceId: "<idHashString>",
			name: "acceleration",
			locationId: "<idHashString>"
		},
		...
		]
	}

### Device Events
Get the recent events for a given device (given its deviceId).

*Note: The JSON response looks just like the "events" section from the device details endpoint and currently includes the last 10 events.*    
[https://graph.api.smartthings.com/api/devices/idHashString/events](https://graph.api.smartthings.com/api/devices/idHashString/events)

**Params**    
`all=<boolean>`    
`max=<number>`    
`source=<boolean>`

### Device Roles
[https://graph.api.smartthings.com/api/devices/idHashString/roles](https://graph.api.smartthings.com/api/devices/idHashString/roles)

### Device Types
Get information about the types of installed devices. This does **not** include any events or readings from the devices. This information is used to populate the SmartThings app dashboard. For example, you will see color hex values for different temperatures which would indicate which color to show based on the current temperature.  If you were to be building your own app, you may find this information useful. It can tell you a lot about each device type. There is a good bit of information in this response.    
[https://graph.api.smartthings.com/api/devicetypes](https://graph.api.smartthings.com/api/devicetypes)

### All Locations
Get information about your locations for all accounts.    
[https://graph.api.smartthings.com/api/locations](https://graph.api.smartthings.com/api/locations)

Example response:

	[
		{
			id: "<idHashString>",
			name: "<yourLocationName>",
			accountId: "<idHashString>",
			latitude: "",
			longitude: "",
			regionRadius: 50,
			backgroundImage: "https://smartthings-location-images.s3.amazonaws.com/standard/standard51.jpg",
			mode: {
				id: "<idHashString>",
				name: "Home"
			},
			modes: [
				{
					id: "<idHashString>",
					name: "Away"
				},
				{
					id: "<idHashString>",
					name: "Home"
				},
				{
					id: "<idHashString>",
					name: "Night"
				}
			],
			permissions: "a"
		}
	]
	
### List All SmartApps
Note: This endpoint will take a little while to load because the response is quite large. It has nothing to do with your hub or account. What it does is it lists all of the available SmartApps that have been created.

It tells you things like the current version of the SmartApp, it's install count, etc. It is essentially an app catalog and it's interesting to browse through it since it contains titles and descriptions for each app. Interestingly enough some of these applications don't even have anything to do with your sensors - though they appear to notify you just the same.

*Note: This call does not require authentication.*    
[https://graph.api.smartthings.com/api/smartapps/](https://graph.api.smartthings.com/api/smartapps/)

### SmartApp Details
This will return information about a particular SmartApp, using its ID which can be found from the above call (or the next call).

*Note: This call does not require authentication.*      
[https://graph.api.smartthings.com/api/smartapps/idHashString](https://graph.api.smartthings.com/api/smartapps/idHashString)

### Your SmartApp Installations
On the otherhand, this endpoint will tell you about the applications you have installed. This call will return some of the same information found in the above SmartApps details call but also contains some information specific to your account. It will include information such as `eventSubscriptions`, location information, and other preferences specifically set by you when you installed/configured the SmartApp.    
[https://graph.api.smartthings.com/api/smartapps/installations/](https://graph.api.smartthings.com/api/smartapps/installations/)

