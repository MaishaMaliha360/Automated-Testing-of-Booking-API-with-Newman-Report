### **Booking API Testing with Postman Newman**
This project showcases API testing with Postman, furnishing a collection of tests to verify different endpoints of the API. 

### **Features**

- Inclusive tests covering GET, POST, PUT, and DELETE requests.
- Comprehensive collection of tests for various API endpoints.
- Environment setup for smooth switching between environments.
- Pre-request scripts for efficient data setup.
- Test scripts for assertion and validation execution.

## API Documentation:
- https://documenter.getpostman.com/view/28052536/2sA35A6QAU
  
### **Technology used:**
- Postman
- Newman

### **Prerequisite:**
- Node Js
- Newman
- Newman Html Report Library

### **Installation**

1. Postman: If you haven't already, [download and install Postman.](https://www.postman.com/downloads/)
2. Clone the repository:
 ```console 
  git clone https://github.com/MaishaMaliha360/Automated-Testing-of-Booking-API-with-Newman-Report.git
```
3. Import the Postman collection:
    - Open Postman.
    - Click on the Import button.
    - Select the file from the repository.
4. Import the Postman environment:
    - In Postman, click on the gear icon in the top right corner.
    - Select **Import** and choose the file.
5. Newman and Report Installation Process:
    - Newman Install Command:
     ```console 
      npm install -g newman
    ```
    - Newman Html Report Install Command:
     ```console 
      npm install -g newman-reporter-htmlextra
    ```
### **Usage**
1. Select Environment:
    -   In Postman, select the appropriate environment (e.g., Development, Production) from the top-right dropdown.
3. Run Collection:
    -   Select the imported collection from the Collections sidebar.
    -   Click on the Runner button to open the collection runner.
    -   Select the desired environment.
    -   Click Start Test to run the collection.
8. View Results:
    -   Once the tests are complete, view the results in the Runner tab.
    -   Detailed test results can be viewed for each request.

## **Testing**

## Test Case Scenarios:

## _**1. Create New Booking**_

### Request URL: https://restful-booker.herokuapp.com/booking/
### Request Method: POST
### Pre-request Script:
```console 
    var firstName = pm.variables.replaceIn("{{$randomFirstName}}")
    pm.environment.set("firstName", firstName)
    console.log("First Name Value "+firstName)
    
    var lastName = pm.variables.replaceIn("{{$randomLastName}}")
    pm.environment.set("lastName", lastName)
    console.log("Last Name Value "+lastName)
    
    var totalPrice = pm.variables.replaceIn("{{$randomInt}}")
    pm.environment.set("totalPrice", totalPrice)
    console.log(totalPrice)
    
    var depositPaid = pm.variables.replaceIn("{{$randomBoolean}}")
    pm.environment.set("depositPaid", depositPaid)
    console.log(depositPaid)
    
    //Date
    const moment = require('moment')
    const today = moment()
    pm.environment.set("checkin", today.add(1,'d').format("YYYY-MM-DD"))
    pm.environment.set("checkout",today.add(5,'d').format("YYYY-MM-DD") )
    
    var additionalNeeds = pm.variables.replaceIn("{{$randomNoun}}")
    pm.environment.set("additionalNeeds", additionalNeeds)

    var checkout = today.add(5,'d').format("YYYY-MM-DD")
    pm.environment.set("checkout", checkout)

    var checkin = today.add(5,'d').format("YYYY-MM-DD")
    pm.environment.set("checkin", checkin)
```
  **Request Body:** 
 ```console 
  {
	  "firstname" : "{{firstName}}",
	  "lastname" : "{{lastName}}",
	  "totalprice" : {{totalPrice}},
	  "depositpaid" : {{depositPaid}},
	  "bookingdates" : {
    	  "checkin" : "{{checkin}}",
    	  "checkout" : "{{checkout}}"
	  },
	  "additionalneeds" : "{{additionalNeeds}}"
  }
```
  **Test:** 
 ```console 
  {
	 var jsonData = pm.response.json()

  pm.environment.set("id", jsonData.bookingid)

  pm.test("Satus code is 200", function(){
    pm.response.to.have.status(200);})
  }
```
  **Response Body:**
 ```console 
  {
    "bookingid": 657,
    "booking": {
        "firstname": "Dasia",
        "lastname": "Grimes",
        "totalprice": 369,
        "depositpaid": true,
        "bookingdates": {
            "checkin": "2024-03-31",
            "checkout": "2024-04-05"
        },
        "additionalneeds": "2"
    }
}
```
 ## _**2. Get Booking Details By ID**_
### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: GET
### Test:
 ```console 
var responseCode = pm.response.code

console.log(responseCode)

if(responseCode==200){

    var jsonData = pm.response.json()
pm.test("First Name Validation", function(){
    pm.expect(pm.environment.get("firstName")).to.eql(jsonData.firstname)

})

pm.test("Last Name Validation", function(){
    pm.expect(pm.environment.get("lastName")).to.eql(jsonData.lastname)

})

pm.test("CheckIn Date Validation", function(){
    pm.expect(pm.environment.get("checkin")).to.eql(jsonData.bookingdates.checkin)

})

pm.test("CheckOut Date Validation", function(){
    pm.expect(pm.environment.get("checkout")).to.eql(jsonData.bookingdates.checkout)

})

pm.test("Additional Needs Validation", function(){
    pm.expect(pm.environment.get("additionalneeds")).to.eql(jsonData.additionalneeds)

})

pm.test("Total Price Validation", function(){
    pm.expect(pm.environment.get("totalprice")).to.eql(jsonData.totalprice.toString())

})

pm.test("Deposit Paid Validation", function(){
    pm.expect(pm.environment.get("dePositpaid")).to.eql(jsonData.depositpaid.toString())

})


/*pm.test("Satus code is 200", function(){
    pm.response.to.have.status(200);
});*/


}else if(responseCode==404){
    pm.test("Not Found")
}else if(responseCode==500){
    pm.test("Server Error")
}else{
    pm.test("Something went wrong. Please check it")
}}
```
**Response Body:**
 ```console 
  {
    "firstname": "Dasia",
    "lastname": "Grimes",
    "totalprice": 369,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-03-31",
        "checkout": "2024-04-05"
    },
    "additionalneeds": "2"
}
```
## _**3. Create A Token For Authentication.**_
### Request URL: https://restful-booker.herokuapp.com/auth
### Request Method: POST
### Pre-request Script: None
### Request Body:
 ```console 
{
	"username": "admin",
	"password": "password123"
}
```
  **Response Body:**
 ```console 
{
    "token": "90e6de3a190e95a"
}
```

 ## _**4. Update the Booking Details**_
### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: PUT
### Pre-request Script:
```console 
    var firstName = pm.variables.replaceIn("{{$randomFirstName}}")
console.log(firstName)
pm.environment.set("Update firstName", firstName)

var lastName = pm.variables.replaceIn("{{$randomLastName}}")
console.log(lastName)
pm.environment.set("Update lastName", lastName)


//Date
const date = require('moment')
const today = date()


//for future date
var checkin = today.add(5,'d').format("YYYY-MM-DD")
//console.log(checkin)

//for previous date
//var checkinPast = today.subtract(5,'d').format("YYYY-MM-DD")
//console.log(checkinPast)
pm.environment.set("Update checkin", checkin)

var checkout = today.add(5,'d').format("YYYY-MM-DD")
pm.environment.set("Update checkout", checkout)


var additionalneeds = pm.variables.replaceIn("{{$randomAlphaNumeric}}")
console.log(additionalneeds)
pm.environment.set("Update additionalneeds", additionalneeds)

var totalprice = pm.variables.replaceIn("{{$randomInt}}")
console.log(totalprice)
pm.environment.set("Update totalprice", totalprice)

var depositpaid = pm.variables.replaceIn("{{$randomBoolean}}")
console.log(depositpaid)
pm.environment.set("Update depositpaid", depositpaid)
```
  **Request Body:** 
 ```console 
  {
	"firstname" : "{{Update firstName}}",
	"lastname" : "{{Update lastName}}",
	"totalprice" : "{{Update totalprice}}",
	"depositpaid" : {{Update depositpaid}},
	"bookingdates" : {
    	"checkin" : "{{Update checkin}}",
    	"checkout" : "{{Update checkout}}"
	},
	"additionalneeds" : "{{Update additionalneeds}}"
}
```
  **Response Body:**
 ```console 
  {
    "firstname": "Armand",
    "lastname": "Lind",
    "totalprice": 218,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-03-31",
        "checkout": "2024-04-05"
    },
    "additionalneeds": "a"
}
```

 ## _**5.Get the Updated Booking Details**_

### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: GET
### Response Body: None 
### Test:
 ```console 
var responseCode = pm.response.code

console.log(responseCode)

if(responseCode==200){

    var jsonData = pm.response.json()
pm.test("First Name Validation", function(){
    pm.expect(pm.environment.get("Update firstName")).to.eql(jsonData.firstname)

})

pm.test("Last Name Validation", function(){
    pm.expect(pm.environment.get("Update lastName")).to.eql(jsonData.lastname)

})

pm.test("CheckIn Date Validation", function(){
    pm.expect(pm.environment.get("Update checkin")).to.eql(jsonData.bookingdates.checkin)

})

pm.test("CheckOut Date Validation", function(){
    pm.expect(pm.environment.get("Update checkout")).to.eql(jsonData.bookingdates.checkout)

})

pm.test("Additional Needs Validation", function(){
    pm.expect(pm.environment.get("Update additionalneeds")).to.eql(jsonData.additionalneeds)

})

pm.test("Total Price Validation", function(){
    pm.expect(pm.environment.get("Update totalprice")).to.eql(jsonData.totalprice.toString())

})

pm.test("Deposit Paid Validation", function(){
    pm.expect(pm.environment.get("Update depositpaid")).to.eql(jsonData.depositpaid.toString())

})


pm.test("Satus code is 200", function(){
    pm.response.to.have.status(200);
});


}else if(responseCode==404){
    pm.test("Not Found")
}else if(responseCode==500){
    pm.test("Server Error")
}else{
    pm.test("Something went wrong. Please check it")
}
```
**Response Body:**
 ```console 
  {
    "firstname": "Armand",
    "lastname": "Lind",
    "totalprice": 218,
    "depositpaid": true,
    "bookingdates": {
        "checkin": "2024-03-31",
        "checkout": "2024-04-05"
    },
    "additionalneeds": "a"
}
```
## _**6.Delete Booking Details**_

### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: DELETE
### Pre-requisite Script : None
### Response Body: None

## _**6.Deleted Booking Check**_

### Request URL: https://restful-booker.herokuapp.com/booking/bookingid
### Request Method: Get
### Pre-requisite Script : None
### Response Body: None
### Test:
 ```console 
var responseCode = pm.response.code

console.log(responseCode)

if(responseCode==200){

   
}else if(responseCode==404){
    pm.test("Delete Successful")
}else if(responseCode==500){
    pm.test("Server Error")
}else{
    pm.test("Something went wrong. Please check it")
}
```
## Run Command:  
- Run Command for Console: 
```console 
newman run Batch24.postman_collection.json -e Batch24.postman_environment.json 
```
- Run Command for Report: 
```console 
newman run Batch24.postman_collection.json -e Batch24.postman_environment.json -r cli,htmlextra
```

## Newman Report Summary:
![1](https://github.com/MaishaMaliha360/Automated-Testing-of-Booking-API-with-Newman-Report/assets/69841337/fc4b642a-922a-47a0-8583-6fbf386ee89e)
![2](ht![3](https://github.com/MaishaMaliha360/Automated-Testing-of-Booking-API-with-Newman-Report/assets/69841337/31c075d9-01e8-4345-b68a-acd7700819d0)
![3](https://github.com/MaishaMaliha360/Automated-Testing-of-Booking-API-with-Newman-Report/assets/69841337/811766b1-096f-4655-9688-4fc498ac0a92)
![4](https://github.com/MaishaMaliha360/Automated-Testing-of-Booking-API-with-Newman-Report/assets/69841337/d1ca3856-05b2-4942-8f98-5bbc9b87b43c)
