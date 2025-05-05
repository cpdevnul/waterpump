This project is to create a solar powered stand alone smart irrigation system using off the shelf and inexpensive materials.  Mine is designed to control a 12v water pump from a reservoir of water (like a water tank or large bucket).  This could easily be adapted to control a valve in normally closed mode, using pressurized city water supplies.  It could be further adapted to simply run on AC power with no need for the battery or charge controller.

Parts list:
1.  45W solar panel with MC4 connectors (a larger size is fine, depends on your battery capacity)
2.  Victron solar charge controller (any charge controller will do, but like this one:  https://www.amazon.com/dp/B075NTT8GH
3.  12V lifepo4 battery, 20ah (more or less may suit your needs).  https://www.amazon.com/dp/B095GYPBTW
4.  One arduino Uno board (https://www.aliexpress.us/item/3256807149536177.html?spm=a2g0o.productlist.main.1.42160oro0oropv&algo_pvid=dc335ff2-6b04-4492-9550-451578e18d39&algo_exp_id=dc335ff2-6b04-4492-9550-451578e18d39-0&pdp_ext_f=%7B%22order%22%3A%223996%22%2C%22eval%22%3A%221%22%7D&pdp_npi=4%40dis%21USD%213.25%213.02%21%21%213.25%213.02%21%402103146f17412137782628623e0a30%2112000040315943283%21sea%21US%21877532669%21X&curPageLogUid=zj3PBTqFdJDS&utparam-url=scene%3Asearch%7Cquery_from%3A)
5.  Various jumpers, large and small, male/female connectors
6.  12v water pump, choose your capacity based upon the area and flow you need:  https://www.amazon.com/dp/B0CKRY3G8Q?ref_=ppx_hzsearch_conn_dt_b_fed_asin_title_1
7. Some shrink tubing to make connections tight:  https://www.amazon.com/dp/B01MFA3OFA
8. power connectors from your power/brain box to the pump enclosure.  I like these but any gauge that can handle your pump will do:  https://www.amazon.com/dp/B07WDBLFDN
9. Capacitive soil moisture sensor - make sure to get corrosion proof versions like these:  https://www.amazon.com/dp/B0BTHL6M19?ref_=ppx_hzsearch_conn_dt_b_fed_asin_title_1
10. 12V to USB power connector (if you have a battery involved) :  https://www.amazon.com/dp/B0C85Q9533
11. some assortment of lever wire connectors, to join a couple load and ground wires.  You can use other things but these just work well:  https://www.harborfreight.com/compact-lever-wire-connector-assortment-25-piece-70056.html
12. Tubing/hoses to match the water pump you choose.  This may not be needed if you are just using valves for pressurized water.
13. Single channel relay like this: https://a.co/d/gGNozj5
14. various lengths of scrap wire, that is an appropriate gauge for the voltage and amperage you intend to use


If you are using an ESP32 and want to attach to wifi and upload data to Google Sheets

Create an IFTTT Account: Go to ifttt.com and sign up for a free account if you don't have one.
Create a New Applet: Click on "Create" or "My Applets" -> "New Applet".   
Configure the Trigger ("IF This"):
Click on the "+ This" button.
Search for the service called Webhooks.
Select the "Receive a web request" trigger.
Choose an Event Name. This name is crucial as it will be part of the URL your ESP32 sends data to. Make it descriptive, for example: esp32_log or plant_data. Remember this name exactly.
Click "Create trigger".
Configure the Action ("Then That"):
Click on the "+ That" button.
Search for the service Google Sheets.
Select the "Add row to spreadsheet" action.
Connect Google Sheets: If you haven't used Google Sheets with IFTTT before, you'll need to click "Connect" and authorize IFTTT to access your Google account and manage your spreadsheets. Grant the necessary permissions.
Configure Action Fields:
Spreadsheet name: Enter the name you want for your Google Sheet (e.g., ESP32 Watering Data IFTTT). IFTTT can create this sheet if it doesn't exist in your Google Drive root folder.
Formatted row: This is where you define what data goes into which column. The Webhook trigger provides "Ingredients" like {{EventName}}, {{Value1}}, {{Value2}}, {{Value3}}, and {{OccurredAt}}. You combine these to form a row. A good format might be: {{OccurredAt}} || {{Value1}} || {{Value2}} || {{Value3}} (The || acts as a column separator for Google Sheets).
{{OccurredAt}}: Timestamp when IFTTT received the request.
{{Value1}}: We can use this for the eventType ("moisture" or "pumpOff").
{{Value2}}: We can use this for the moisture percentage.
{{Value3}}: We can use this for the pump duration.
  
Drive folder path (optional): Specify a folder in Google Drive if you don't want the sheet created in the root.
Click "Create action".
Review and Finish: Give your Applet a title (optional) and click "Finish".
Get Your Webhook URL and Key:
Go to ifttt.com/maker_webhooks.
Click on the "Documentation" button.
You will see your unique Key. Copy this key carefully.
You'll also see the URL structure: https://maker.ifttt.com/trigger/{event}/with/key/{your_key}
Replace {event} with the Event Name you chose in step 3 (e.g., esp32_log).
Replace {your_key} with the key you just copied.
This complete URL is what your ESP32 will need to send data to.
Part 2: Google Sheets Setup

IFTTT will likely create the spreadsheet specified in step 4.
Open the sheet once it's created. You should see columns based on your "Formatted row" definition. Add header names manually to the first row if IFTTT didn't (e.g., "Timestamp", "EventType", "Moisture (%)", "Pump Duration (s)").
