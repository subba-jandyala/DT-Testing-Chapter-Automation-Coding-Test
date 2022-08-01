# DT-Testing-Chapter-Automation-Coding-Test
DT Testing Chapter Automation Coding Test

Assignment
Given the URL http://todomvc.com/examples/vue/ translate the user stories into different
scenarios Gherkins Given, When, Then 

User Story
In order to remember the things I want to do, as a ToDo MVC user,
I want to manage my todo list

Suggestions
1. Think of different scenarios for this user story
2. Decide on a framework (e.g., Cucumber, Specflow, etc.), and driver (e.g., Selenium API webdriver
library, etc.)


# Solution
# pre-condition --> Empty to do list

# Step1 Identify the basic information
Add items to ToDo list
Validate the items added to the ToDo list (usiong Radio buttons)
Validate the status of items (i items left --> pending items)
Validate the lists filter with All, Active, Completed 
Validate the lists filter for clear completed

# Step2 Generate test data combinations with the above basic information
Using All pair testing technique we will find out the combinations of the tests and remove the irrelavant combinations in the tests

![image](https://user-images.githubusercontent.com/110383824/182114845-f728b6ac-f075-41c7-a537-64b1d42c961b.png)

Valid Combinations

	Item	ItemsLeftPending	ItemStatus
1	Empty	None	All
2	Empty	None	Active
3	Empty	None	Completed
4	Empty	None	Clear Completed
5	Item1	None	All
6	Item1	None	Active
7	Item1	None	Completed
8	Item1	None	Clear Completed
9	Item1	Select	All
10	Item1	Select	Active
11	Item1	Select	Completed
12	Item1	Select	Clear Completed
13	Item1	UnSelect	All
14	Item1	UnSelect	Active
15	Item1	UnSelect	Completed
16	Item1	UnSelect	Clear Completed
17	Item2	None	All
18	Item2	None	Active
19	Item2	None	Completed
20	Item2	None	Clear Completed
21	Item2	Select	All
22	Item2	Select	Active
23	Item2	Select	Completed
24	Item2	Select	Clear Completed
25	Item2	MoreThanOneSelect	All
26	Item2	MoreThanOneSelect	Active
27	Item2	MoreThanOneSelect	Completed
28	Item2	MoreThanOneSelect	Clear Completed
![image](https://user-images.githubusercontent.com/110383824/182115376-bc6bb980-93f5-4c8d-a821-e5f6a6be810b.png)


# Scenarios covering the above test data combiantions including adding, removing and editing the items from the list

Scenario: Empty list can have item added
Given an empty Todo list
When I add a Todo for ‘TaskItem1’
Then only that item is listed
And the list summary is ‘1 item left’
And the list’s filter is set to ‘All’ with ‘Completed’ & ‘Active’ unset
And ‘Clear completed’ is unavailable


Scenario: Empty list can have two items added
Given an empty Todo list
When I add Todos for ‘TaskItem1’ & ‘TaskItem2’
Then only those items are listed
And the list summary is ‘2 items left’
And the list’s filter is set to ‘All’ with ‘Completed’ & ‘Active’ unset
And ‘Clear completed’ is unavailable


Scenario: Item completion changes the list
Given a Todo list with items ‘TaskItem1’ & ‘TaskItem2’
When the first item is marked as complete
Then only the second item is listed as active
And the list summary is ‘1 item left’
And ‘Clear completed’ is available

Scenario: Completed items should not be visible in active filter
Given a Todo list with items ‘TaskItem1’ & ‘TaskItem2’
And the first item is completed
When the filter is set to ‘Active’
Then only the second item is listed
And the list summary is ‘1 item left’
And ‘Clear completed’ is available
And the route is /active


Scenario: All completed items should not be visible in active filter
Given a Todo list with items ‘TaskItem1’ & ‘TaskItem2’
And the first item is completed
And the second item is completed
When the filter is set to ‘Active’
Then nothing is listed
And the list summary is ‘0 items left’
And ‘Clear completed’ is available
And the route is /active



Scenario: Uncompleted items should not be visible in the completed filter
Given a Todo list with items ‘TaskItem1’ & ‘TaskItem2’
And the first item is completed
When the filter is set to ‘Completed’
Then only the first item is listed
And the list summary is ‘1 item left’
And ‘Clear completed’ is available
And the route is /completed


Scenario: Items can be cleared
Given a Todo list with items ‘TaskItem1’ & ‘TaskItem2’
And the first item is completed
When ‘Clear completed’ is clicked
Then only the second item is listed
And the list summary is ‘1 item left’


Scenario: clear all works when none completed
Given a Todo list with items ‘TaskItem1’ & ‘TaskItem2’
When the clear all items affordance is toggled
Then both items are listed
But nothing listed is active
And the list summary is ‘0 items left’



Scenario: clear all works when one of two completed
Given a Todo list with items ‘TaskItem1’ & ‘TaskItem2’
And the first item is completed
When the clear all items affordance is toggled
Then both items are listed
But nothing listed is active
And the list summary is ‘0 items left’



Scenario: Clear all un-clears when two of two completed
Given a Todo list with items ‘TaskItem1’ & ‘TaskItem2’
And the first item is completed
And the second item is completed
When the clear all items affordance is toggled
Then both items are listed
And both are active
And the list summary is ‘2 items left’



Scenario: Items can be straight removed
Given a Todo list with a single item ‘TaskItem1’
When the item is removed
Then nothing is listed



Scenario: Initiation of editing takes away delete and complete affordances
Given a Todo list with single item ‘TaskItem1’
When the item is selected for edit
Then the item cannot be completed
And the item cannot be removed


Scenario: Editing can be abandoned
Given a Todo list with a single item ‘TaskItem1’
And that item is being edited
When editing is abandoned
Then only that item is listed
And the list summary is ‘1 item left’
