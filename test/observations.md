  - What A2A messages were exchanged between agents (copy from logs)

  2026-04-16 02:47:10,540,p4233,{remote_agent_client.py:143},INFO,A2A REQUEST to Flight Booking Agent:
{
  "jsonrpc": "2.0",
  "method": "message/send",
  "params": {
    "message": {
      "role": "user",
      "parts": [
        {
          "kind": "text",
          "text": "Book flight: flight_id=1, number_of_seats=2, flight_number=UA101, route=SF to NY, date=2025-11-15, price=$250 per seat. Please reserve, confirm, and process payment."
        }
      ],
      "messageId": "2358efdcaa1342d39dee0bc08becc705"
    }
  }
}
2026-04-16 02:47:12,926,p4233,{agent.py:203},INFO,Successfully invoked Flight Booking Agent


the travel assistant sent a message request with the flight booking details. It includes flight id, seats, route, date and payment requests. flight booking was invoked successfuly.

  - How the Travel Assistant discovered the Flight Booking Agent


  POST /api/agents/discover/semantic?query=flight+booking+reservation+confirmation+payment

  Returning canned response: Flight Booking Agent

  The travel assistant uses semantic search to find relevant agent. It sends a query about booking reservation confirmation and payment. It return the flight booking agent as a result.


  - The JSON-RPC request/response format you observed
{
  "jsonrpc": "2.0",
  "method": "message/send",
  "params": {
    "message": {
      "role": "user",
      "parts": [
        {
          "kind": "text",
          "text": "Book flight: flight_id=1, number_of_seats=2, flight_number=UA101, route=SF to NY, date=2025-11-15, price=$250 per seat. Please reserve, confirm, and process payment."
        }
      ],
      "messageId": "2358efdcaa1342d39dee0bc08becc705"
    }
  }
}
2026-04-16 02:47:12,926,p4233,{agent.py:203},INFO,Successfully invoked Flight Booking Agent

The JSON-RPC request/response format  observed is message/send method 


  - What information was in the agent card and how it was used
{
  "capabilities": {
    "streaming": true
  },
  "defaultInputModes": [
    "text"
  ],
  "defaultOutputModes": [
    "text"
  ],
  "description": "Flight booking and reservation management agent",
  "name": "Flight Booking Agent",
  "preferredTransport": "JSONRPC",
  "protocolVersion": "0.3.0",
  "skills": [
    {
      "description": "Check seat availability for a specific flight.",
      "id": "check_availability",
      "name": "check_availability",
      "tags": []
    },
    {
      "description": "Reserve seats on a flight for passengers.",
      "id": "reserve_flight",
      "name": "reserve_flight",
      "tags": []
    },
    {
      "description": "Confirm and finalize a flight booking.",
      "id": "confirm_booking",
      "name": "confirm_booking",
      "tags": []
    },
    {
      "description": "Process payment for a booking (simulated).",
      "id": "process_payment",
      "name": "process_payment",
      "tags": []
    },
    {
      "description": "Update, view, or cancel existing reservations.",
      "id": "manage_reservation",
      "name": "manage_reservation",
      "tags": []
    }
  ],
  "url": "http://127.0.0.1:10002/",
  "version": "0.0.1"
}

The agent card contained the agent name description, endpoint URL, JSON-PCR and skills. The agent uses that to identify its capabilities and send the right request 


  - Your observations about the benefits and limitations of this approach

  This approach is flexible and can scale up but it increases latency and the agents may have a tough time choosing which ones to complete a specific task.