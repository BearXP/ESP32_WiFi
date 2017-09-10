Google home just runs python code when a voice command is recognised.
	

How to setup Google Home Assist:
	https://www.hackster.io/Salmanfarisvp/googlepi-google-assistant-on-raspberry-pi-9f3677
	https://www.raspberrypi.org/forums/viewtopic.php?t=188958

How to setup voice commands:
	https://github.com/Tom-Archer/gmusicaiy
	https://github.com/simon-weber/gmusicapi
	

Supla uses the RestFUL API
	https://github.com/SUPLA/restful-api-client/blob/master/php/SuplaCloudClient.php
	https://github.com/SUPLA/restful-api-client/blob/master/php/example.php
	https://github.com/SUPLA/api-client-php
		Then create an instance of a client in your code.
			<?php
			$client = new \Supla\ApiClient\SuplaApiClient([
			    'server' => 'svrX.supla.org',
			    'clientId' => 'YOUR_CLIENT_ID',
			    'secret' => 'YOUR_SECRET',
			    'username' => 'YOUR_USERNAME',
			    'password' => 'YOUR_PASSWORD',
			]);
		Usage: Getting server info
			$client->getServerInfo();

Example interfacing with the RestFUL API in Python:
	https://www.codementor.io/sagaragarwal94/building-a-basic-restful-api-in-python-58k02xsiq
from flask import Flask, request
from flask_restful import Resource, Api
from sqlalchemy import create_engine
from json import dumps
from flask.ext.jsonpify import jsonify

db_connect = create_engine('sqlite:///chinook.db')
app = Flask(__name__)
api = Api(app)

class Employees(Resource):
    def get(self):
        conn = db_connect.connect() # connect to database
        query = conn.execute("select * from employees") # This line performs query and returns json result
        return {'employees': [i[0] for i in query.cursor.fetchall()]} # Fetches first column that is Employee ID

class Tracks(Resource):
    def get(self):
        conn = db_connect.connect()
        query = conn.execute("select trackid, name, composer, unitprice from tracks;")
        result = {'data': [dict(zip(tuple (query.keys()) ,i)) for i in query.cursor]}
        return jsonify(result)

class Employees_Name(Resource):
    def get(self, employee_id):
        conn = db_connect.connect()
        query = conn.execute("select * from employees where EmployeeId =%d "  %int(employee_id))
        result = {'data': [dict(zip(tuple (query.keys()) ,i)) for i in query.cursor]}
        return jsonify(result)
        

api.add_resource(Employees, '/employees') # Route_1
api.add_resource(Tracks, '/tracks') # Route_2
api.add_resource(Employees_Name, '/employees/<employee_id>') # Route_3


if __name__ == '__main__':
     app.run(port='5002')