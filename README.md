# travel-app
from flask import Flask, request, jsonify

app = Flask(__name__)

# Sample data for destinations
destinations = [
    {"id": 1, "name": "Paris", "country": "France", "description": "The city of love and lights."},
    {"id": 2, "name": "Tokyo", "country": "Japan", "description": "A bustling metropolis with a mix of tradition and technology."},
    {"id": 3, "name": "New York", "country": "USA", "description": "The city that never sleeps."}
]

@app.route("/destinations", methods=["GET"])
def get_destinations():
    return jsonify(destinations)

@app.route("/destination/<int:destination_id>", methods=["GET"])
def get_destination(destination_id):
    destination = next((d for d in destinations if d["id"] == destination_id), None)
    if destination:
        return jsonify(destination)
    return jsonify({"error": "Destination not found"}), 404

@app.route("/add_destination", methods=["POST"])
def add_destination():
    new_destination = request.get_json()
    new_destination["id"] = len(destinations) + 1
    destinations.append(new_destination)
    return jsonify(new_destination), 201

if __name__ == "__main__":
    app.run(debug=True)
