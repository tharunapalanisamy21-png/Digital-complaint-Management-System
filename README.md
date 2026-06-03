# Digital-complaint-Management-System
from flask import Flask, request, jsonify

app = Flask(__name__)

complaints = []

# Add Complaint
@app.route('/complaints', methods=['POST'])
def add_complaint():
    data = request.json

    complaint = {
        "id": len(complaints) + 1,
        "name": data["name"],
        "issue": data["issue"],
        "status": "Pending"
    }

    complaints.append(complaint)

    return jsonify({
        "message": "Complaint Registered Successfully",
        "complaint": complaint
    }), 201

# View Complaints
@app.route('/complaints', methods=['GET'])
def get_complaints():
    return jsonify(complaints)

# Update Complaint Status
@app.route('/complaints/<int:complaint_id>', methods=['PUT'])
def update_status(complaint_id):

    for complaint in complaints:
        if complaint["id"] == complaint_id:
            complaint["status"] = request.json["status"]

            return jsonify({
                "message": "Status Updated Successfully",
                "complaint": complaint
            })

    return jsonify({"error": "Complaint Not Found"}), 404

if __name__ == '__main__':
    app.run(debug=True)
