Trello API - Postman Testing

This project consists of Postman collections and requests designed to interact with the Trello API. It includes the workflow for creating, updating, retrieving, deleting, and archiving various Trello entities such as boards, lists, cards, and checklists. The requests are structured in a way to automate and test the full lifecycle of each entity.

Environment Variables

The following environment variables are used in this project:
	•	base_url: https://api.trello.com
	•	key: Your Trello API key (private key).
	•	token: Your Trello API token (private token).

Collections Overview

Folder 1 - Board

Requests Work Flow:
	1.	POST: Create a new board on Trello
	2.	GET: Confirm the board was successfully created
	3.	PUT: Update the board’s name
	4.	GET: Confirm the updated board’s name
	5.	PUT: Send an invitation by email to collaborate
	6.	GET: Get all memberships of the board

Tests:
	•	Status code should be 200.
	•	Board name is confirmed after the PUT request.

Folder 2 - List inside the New Created Board

Requests Work Flow:
	1.	POST: Create a new list inside the new board
	2.	GET: Confirm the list was successfully created
	3.	PUT: Update the list’s name
	4.	GET: Confirm all the lists inside the board
	5.	GET: Filter lists inside the board based on their status
	6.	PUT: Archive or unarchive a list
	7.	GET: Confirm the list was successfully archived

Tests:
	•	Status code should be 200.
	•	List name is confirmed after the PUT request.

Folder 3 - Cards Inside the New List

Requests Work Flow:
	1.	POST: Create a new card inside the new list
	2.	GET: Confirm the card was successfully created
	3.	GET: Confirm all the cards inside the list
	4.	PUT: Update the card’s name, color, and description
	5.	GET: Confirm the updated card
	6.	POST: Add a comment to the new card
	7.	GET: Filter cards based on their status

Tests:
	•	Status code should be 200.
	•	Card name is confirmed after the PUT request.

Folder 4 - Checklist and Check Items Inside the New Card

Requests Work Flow:
	1.	POST: Create a checklist inside the new card
	2.	GET: Confirm the checklist was successfully created
	3.	PUT: Update the checklist’s name
	4.	GET: Confirm the updated checklist
	5.	POST: Create a checklist item inside the checklist
	6.	GET: Confirm the checklist item was successfully created

Tests:
	•	Status code should be 200.
	•	Checklist and check item names are confirmed after the PUT request.

Folder 5 - Delete Items and Confirmation

Requests Work Flow:
	1.	DELETE: Delete the last checklist item created inside the checklist
	2.	GET: Confirm the checklist item was successfully deleted
	3.	DELETE: Delete the last checklist inside the card
	4.	GET: Confirm the checklist was successfully deleted
	5.	DELETE: Delete the last card inside the list
	6.	GET: Confirm the card was successfully deleted

GET Requests after Delete:
	•	To avoid errors, a delay is added between the DELETE and GET requests to confirm successful deletion (using setTimeout).

setTimeout(function() {
    pm.sendRequest({
        url: '{{base_url}}/1/checklists/{{checklist_id}}/checkItems/{{checkitem_id}}?key={{key}}&token={{token}}',
        method: 'GET',
    }, function(err, res) {
        pm.test("Verify that CheckItem was deleted", function() {
            pm.response.to.have.status(404);  // Resource should no longer exist
        });
    });
}, 3000); // Delay of 3 seconds

Folder 6 - Archive Cards

Requests Work Flow:
	1.	POST: Create a new card inside the list
	2.	GET: Confirm new card
	3.	POST: Archive all the cards inside the list

Tests:
	•	Status code should be 200.
	•	Card names are confirmed after the PUT request.

Folder 7 - Last Item to Delete (Board) and Confirmation

Requests Work Flow:
	1.	DELETE: Delete the entire board on Trello
	2.	GET: Confirm the board was successfully deleted

Tests:
	•	Status code should be 200 after deletion.
	•	The last GET request confirms that the board was successfully deleted (404 is expected).

How to Run the Tests
	1.	Import the Collection:
	•	Download and import the Postman collection and environment into your Postman app.
	2.	Set up the Environment:
	•	Set the environment variables (base_url, key, token) with your Trello API credentials.
	3.	Run Requests:
	•	You can run each request individually or use Postman’s collection runner to execute all requests in the collection.
	4.	Review Test Results:
	•	After running the requests, Postman will automatically display the results of the tests in the Test Results tab.

Conclusion

This Postman project automates testing the entire Trello API lifecycle, ensuring that boards, lists, cards, and checklists are created, updated, and deleted properly. You can extend this project further to include additional Trello API functionalities or integrate it into your continuous integration/continuous deployment (CI/CD) pipeline for automated testing.