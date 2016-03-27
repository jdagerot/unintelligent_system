# unintelligent_system

## Mitt ointelligenta hemautomatiseringssystem

### Systemet består av följande hårdvara:

-Diverse zwave relän
-En zwave sladdbrytare
-Diverse telldus relän
-Diverse telldus brytare

-2 Raspberry PI (2&3)
-Ett RazBerry kontrollkort
-En tellstick DUO
-En 7" touchskärm

###Tjänster
På RPi3 körs fyra tjänster:

-node sensorCollecting
-node tellstickcontrol
-node webserver
-mongoDB

På RPi2 körs bara en webläsare i kioskläge som är hårdkodad att gå mot RPi3:an. (Webservern)

_sensorCollecting_ lyssnar sensordata och sparar i databasen. Den är fristående eftersom de andra servrarna jobbar jag en del med så de tas upp och ner ganska ofta.

_tellstickcontrol_ lyssnar på knapptryckningar från tellsticken och släcker/tänder som den skall, den uppdaterar också databasen. Dock berättar den inte för webservern att någonting hänt (ännu)

_webservern_ servar alla klienter, i huvudsak RPi, det är här den mesta logiken ligger. Den sätter upp en socket.io mot alla anslutna klienter och skickar all information genom meddelanden i socketen. När en klient tänder en lampa till exempel så tänds lampan och ett meddelande broadcastas till alla sockets om att lampans status uppdaterats. Det är för när detta meddelande kommer som ikonen i gränssnittet byter färg. 

###zwave/telldus
Jag har en keywordlista med alla kända enheter oavsett protocol, just nu ser listan ungefär ut såhär:

{
	"zwave" : {
		1 : {
			node_id: 1,
			value: false
		}
	},
	telldus : {
		1 : {
			node_id: 1,
			value: true,
			dimlevel : 128
		}
	} 
}

När jag sparar undan listan i databasen formaterar jag om den lite så en record ser ut så här i mongoDB:

{
	_id : {
		protocol : "zwave",
		node_id : 1
	},
	value: false,
}

Jag skall skriva en ny modul för hanteringen av in-memory-listan, just nu är det väldigt nära exempelkoden som kom med node-openzwave-shared.

###HTML
Websidan är vanlig HTML5 med bootstrap och ett bootstrap-tema som jag hittade.


