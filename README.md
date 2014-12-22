Running-Weather-App
===================
#http://api.wunderground.com/api/0f356ce80c004a3b/hourly/q/VA/Blacksburg.json

import urllib2
import json
f = urllib2.urlopen('http://api.wunderground.com/api/0f356ce80c004a3b/hourly/q/VA/Blacksburg.json')
json_string = f.read()
parsed_json = json.loads(json_string)
print json.dumps(parsed_json, indent=4, sort_keys=True)
for forecast in parsed_json['hourly_forecast']:
	DP = int(forecast['dewpoint']['english'])
	if DP < 55:
		DP_score = 5
	elif DP < 60:
		DP_score = 4
	elif DP < 65:
		DP_score = 3
	elif DP < 70:
		DP_score = 2
	elif DP < 75:
		DP_score = 1
	else:
		DP_score = 0
	forecast['dewpoint']['score'] = DP_score
	#print forecast['FCTTIME']['pretty'] + " Score: " + str(DP_score)

	FL = int(forecast['feelslike']['english'])
	if FL > 40 and FL <= 50:
		FL_score = 5
	if FL > 30 and FL <= 40:
		FL_score = 4
	if FL > 50 and FL <= 60:
		FL_score = 4
	if FL > 20 and FL <= 30:
		FL_score = 3
	if FL > 60 and FL <= 70:
		FL_score = 3
	if FL > 10 and FL <= 20:
		FL_score = 2
	if FL > 70 and FL <= 80:
		FL_score = 2
	if FL > 0 and FL <= 10:
		FL_score = 1
	if FL > 80 and FL <= 90:
		FL_score = 1
	if FL < 0:
		FL_score = 0
	if FL > 90:
		FL_score = 0
	forecast['feelslike']['score'] = FL_score
	#print forecast['FCTTIME']['pretty'] + " Score: " + str(FL_score)

	WS = int(forecast['wspd']['english'])
	if WS <= 4:
		WS_score = 5
	elif WS <= 6:
		WS_score = 4
	elif WS <= 10:
		WS_score = 3
	elif WS <= 12:
		WS_score = 2
	elif WS <= 18:
		WS_score = 1
	else:
		WS_score = 0
	forecast['wspd']['score'] = WS_score
	#print forecast['FCTTIME']['pretty'] + " Score: " + str(WS_score)
	
	#print forecast['FCTTIME']['pretty'] + "  Condition: " + forecast['condition']
	
	CONDITION = forecast['condition']
	if CONDITION.find("storm") != -1:
		C_score = 0	
	elif CONDITION.find("Sleet") != -1:
		C_score = 0
	elif CONDITION.find("Chance of Rain") != -1:
		C_score = 3 
	elif CONDITION.find("Chance of Showers") != -1:
		C_score = 2
	elif CONDITION.find("Chance of Flurries") != -1:
		C_score = 3
	elif CONDITION.find("Chance of Freezing Rain") != -1: 
		C_score = 1
	elif CONDITION.find("Chance of Sleet") != -1:
		C_score = 1
	elif CONDITION.find("Chance of Snow Showers") != -1:
		C_score = 2
	elif CONDITION.find("Chance of Snow") != -1:
		C_score = 3
	elif CONDITION.find("Chance of Ice") != -1:
		C_score = 1
	elif CONDITION.find("Freezing Rain") != -1:
		C_score = 0
	elif CONDITION.find("Ice") != -1:
		C_score = 0
	elif CONDITION.find("Blizzard") != -1:
		C_score = 0
	elif CONDITION.find("Rain") != -1:
		C_score = 1
	elif CONDITION.find("Showers") != -1:
		C_score = 1
	elif CONDITION.find("Flurries") != -1:
		C_score = 2
	elif CONDITION.find("Haz") != -1:
		C_score = 3
	elif CONDITION.find("Fog") != -1:
		C_score = 2
	elif CONDITION.find("Mostly Cloudy") != -1:
		C_score = 4
	elif CONDITION.find("Cloudy") != -1:
		C_score = 3
	elif CONDITION.find("Mostly Sunny") != -1:
		C_score = 3
	elif CONDITION.find("Partly Cloudy") != -1:
		C_score = 5
	elif CONDITION.find("Partly Sunny") != -1:
		C_score = 4
	elif CONDITION.find("Snow Showers") != -1:
		C_score = 1
	elif CONDITION.find("Snow") != -1:
		C_score = 2
	elif CONDITION.find("Sunny") != -1:
		C_score = 2
	elif CONDITION.find("Overcast") != -1:
		C_score = 4
	elif CONDITION.find("Scattered Clouds") != -1:
		C_score = 2
	elif CONDITION.find("Clear") != -1:
		C_score = 5
	elif CONDITION.find("Very Hot") != -1 or CONDITION.find("Very Cold") != -1:
		C_score = 0
	else:
		C_score = 0
	
	#print forecast['FCTTIME']['pretty'] + " Score: " + str(C_score)

	Total = DP_score + C_score + WS_score + FL_score

	print "The total score for " + forecast['FCTTIME']['pretty'] + " is " + str(Total) + "."
	if C_score == 0 or DP_score == 0:
		if DP_score == 0:
			print "The dew point is " + forecast['dew point']['english']
		if C_score == 0:
			print "The conditions are " + str(forecast['condition'])
		print "Conditions may be dangerous."



f.close()
