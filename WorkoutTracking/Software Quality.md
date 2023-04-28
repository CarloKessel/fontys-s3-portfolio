`   `Software Quality


Carlo van Kessel 

S-DB-IP3 and S-DB-GP3

S3-DB03

Hans van Heumen, Marc van Grootel

09-01-2023

V3



Version control 


|Version|Changes|
| :- | :- |
|1|Initial version. |
|2|Added backend tests.|
|3|Added load testing.|
|||


# Contents
[Frontend	5](#_toc124599886)

[Unit testing	5](#_toc124599887)

[What did I use?	5](#_toc124599888)

[How did I use it?	5](#_toc124599889)

[Results	5](#_toc124599890)

[CI/CD	6](#_toc124599891)

[Integration testing	6](#_toc124599892)

[End-to-end testing	6](#_toc124599893)

[What did I use?	6](#_toc124599894)

[How did I use it?	6](#_toc124599895)

[Results	7](#_toc124599896)

[CI/CD	7](#_toc124599897)

[Performance testing	7](#_toc124599898)

[What did I use?	7](#_toc124599899)

[How did I use it?	7](#_toc124599900)

[Results	8](#_toc124599901)

[Load testing	8](#_toc124599902)

[What did I use?	8](#_toc124599903)

[How did I use it?	8](#_toc124599904)

[Results	9](#_toc124599905)

[CI/CD	9](#_toc124599906)

[Static code analysis	9](#_toc124599907)

[What did I use?	9](#_toc124599908)

[How did I use it?	9](#_toc124599909)

[Results	10](#_toc124599910)

[CI/CD	10](#_toc124599911)

[Backend	11](#_toc124599912)

[Unit testing	11](#_toc124599913)

[What did I use?	11](#_toc124599914)

[How did I use it?	11](#_toc124599915)

[Results	11](#_toc124599916)

[CI/CD	11](#_toc124599917)

[integration testing	12](#_toc124599918)

[What did I use?	12](#_toc124599919)

[How did I use it?	12](#_toc124599920)

[Results	12](#_toc124599921)

[CI/CD	12](#_toc124599922)

[Security testing	12](#_toc124599923)

[Static code analysis	13](#_toc124599924)

[What did I use?	13](#_toc124599925)

[How did I use it?	13](#_toc124599926)

[Results	13](#_toc124599927)

[CI/CD	13](#_toc124599928)

[Test files	13](#_toc124599929)

[Frontend	13](#_toc124599930)

[Backend	13](#_toc124599931)

[Sources	13](#_toc124599932)




# <a name="_user_stories"></a><a name="_toc124599886"></a>Frontend
## <a name="_toc124599887"></a>Unit testing
### <a name="_toc124599888"></a>What did I use? 
For unit testing my Vue code I used Vue Test Utils (VTU). With VTU you can easily make unit tests because it isolates the component. 

### <a name="_toc124599889"></a>How did I use it?
I created a spec.js file in the automatically created tests/unit folder. In this file I can then easily import a component and use the mount from VTU. Now I can make a test: 

***test***('Should render input for a new workout', () => {
`    `//Arrange
`    `const wrapper = mount(***startNewWorkout***)

`    `//Act
`    `const newWorkoutInput = wrapper.get('.form-control')

`    `//Assert
`    `expect(newWorkoutInput.element).toBeTruthy();
})

Here I test if the input on the startNewWorkout component is rendered. With the get I get the input element with its class name called form-control. Then I expect the element to be truth which means it has rendered.

### <a name="_toc124599890"></a>Results
When running all the test with the command: ‘npm run unit:test’. It runs all the tests and gives me the results: 


I added that when it runs it gives me a coverage: 

### <a name="_toc124599891"></a>CI/CD
In my CI I added the unit tests: 

When the tests fail the CI fails and the developer needs to make changes. 

[CI file](https://github.com/WorkoutTracking/Frontend/blob/main/.github/workflows/CI.yml)

## <a name="_toc124599892"></a>Integration testing
In the frontend I don’t have integration tests. This is because you can’t access my application without login in with Keycloak. Since this will then be called an end-to-end test, I don’t have an integration test. 

## <a name="_toc124599893"></a>End-to-end testing
### <a name="_toc124599894"></a>What did I use? 
For end-to-end testing I used Cypress. This allows you to test you application just as a user would. You can test the full application. 

### <a name="_toc124599895"></a>How did I use it?
I created 2 \*.cy.js files, these files are the test files. At the start I use a before each: 

***beforeEach***(() => {
`    `***cy***.visit('http://localhost:8081/workout/5498f676-d9ec-4dba-9e16-164dc782bfa5')

`    `***cy***.get('#username')
.type(***Cypress***.env('CYPRESS\_USERNAME'))

`    `***cy***.get('#password')
.type(***Cypress***.env('CYPRESS\_PASSWORD'))

`    `***cy***.get('#kc-form-login').submit()
})

Here I login into the application. Without logging in you can’t access the application. 

Then I test the application: 

***it***('should update set', function () {
`    `***cy***.get('.setsInput')
.type('2')

`    `***cy***.get('.repsInput')
.type('8')

`    `***cy***.get('.weightInput')
.type('120')

`    `***cy***.get('.restInput')
.type('5')

`    `***cy***.get('.updateSet').submit()

`    `***cy***.get('.setsInput').should('have.value', '2')
`    `***cy***.get('.repsInput').should('have.value', '8')
`    `***cy***.get('.weightInput').should('have.value', '120')
`    `***cy***.get('.restInput').should('have.value', '5')
});

This tests if I can update a set. 
### <a name="_toc124599896"></a>Results
When running all the tests with the command: ‘npm run cypress:test’. It runs all the tests in the folder and here are the results: 


### <a name="_toc124599897"></a>CI/CD
I tried to automate this process in my CD. Unfortunately, I did not get docker to work with my database, Keycloak, and backend. This results in me getting 401 and 403 messages. What I did do is commented it out in my CD. So, when I eventually get docker to work I can just uncommand the end-to-end part in my CD, and it should automatically work. 

[CD file](https://github.com/WorkoutTracking/Frontend/blob/main/.github/workflows/CD.yml)

## <a name="_toc124599898"></a>Performance testing
### <a name="_toc124599899"></a>What did I use? 
For my performance test I used Google Lighthouse. 

### <a name="_toc124599900"></a>How did I use it?
I used it in my CD. I did not write single tests because my application you need to login to even start. So that would then be an end-to-end test.

### <a name="_toc124599901"></a>Results
When running my CD, it generates a report with a link you can go to: 

In this report you can see that PWA is not installable. This is because it links to Keycloak: [Report](https://storage.googleapis.com/lighthouse-infrastructure.appspot.com/reports/1672235395682-99280.report.html). But when I did a run earlier, when Keycloak was down I get this: [Report](https://storage.googleapis.com/lighthouse-infrastructure.appspot.com/reports/1672234972173-10790.report.html). Here you can see that PWA is installable. 

## <a name="_toc124599902"></a>Load testing
### <a name="_toc124599903"></a>What did I use? 
For load testing I used Artillery. Artillery is the most advanced load testing in the world and is used by big companies like Gymshark, Brave, and Amity. 

### <a name="_toc124599904"></a>How did I use it?
With Artillery it is easy to make tests. I installed Artillery and created an yml file: 

config:
`  `target: "http://localhost:8081"
`  `phases:
`    `- duration: 60
`      `arrivalRate: 5
`      `name: Warm up
`    `- duration: 120
`      `arrivalRate: 5
`      `rampTo: 50
`      `name: Ramp up load
`    `- duration: 600
`      `arrivalRate: 50
`      `name: Sustained load
`  `ensure:
`    `maxErrorRate: 1
`    `max: 500

scenarios:
`  `- name: "Go to home"
`    `flow:
`      `- get:
`          `url: "/"

This script will go to the home page. It has 3 phases: warm up, ramp up, and sustained. The warmup phase is a phase where the site is under normal load. The ramp up phase is a more loaded phase where the site is more tested. The sustained phase is a phase which runs longer for more reliability. If there is one error the test fails. 

### <a name="_toc124599905"></a>Results


### <a name="_toc124599906"></a>CI/CD
I have not added this test to my CI, this is because then my CI would take about 20 minutes. Now I can run this local in the background. 

## <a name="_toc124599907"></a>Static code analysis 
### <a name="_toc124599908"></a>What did I use? 
For my static code analysis, I used SonarCloud. SonarCloud is a code analysis platform that helps with detecting code quality and security issues within code.  

### <a name="_toc124599909"></a>How did I use it?
To install sonarcloud you need to add a sonar-project.properties file. Here you fill in some credentials: 

sonar.projectKey=WorkoutTracking\_Frontend
sonar.organization=workouttracking
sonar.projectName=Frontend
sonar.host.url=https://sonarcloud.io

### <a name="_toc124599910"></a>Results

Here you can see all the results of the scan. 

The coverage does not work. This is because you need to make report files of the tests. It tried to get it to work but SonarCloud would not recognize my coverage files. So that is way is show coverage in my unit test above.

### <a name="_toc124599911"></a>CI/CD
I automated this scan in my CI: 

Then in my SonarCloud dashboard I can see the last scan. 


# <a name="_toc124599912"></a>Backend
## <a name="_toc124599913"></a>Unit testing
### <a name="_toc124599914"></a>What did I use? 
For unit testing I used the test implementation of Junit. With @QuarkusTest above the class its recognized as a test class.

### <a name="_toc124599915"></a>How did I use it?
@Test
@TestSecurity(user = "carlovankessel@yahoo.nl", roles = "user")
public void When\_Get\_Workouts\_By\_User\_Is\_More\_Than\_Zero() {
`    `// Arrange
`    `String email = "carlovankessel@yahoo.nl";
`    `int lowestSize = 0;

`    `// Act
`    `List<Workout> workouts = workoutService.allWorkoutsByUser(email);

`    `// Assert
`    `Assertions.*assertNotEquals*(lowestSize, workouts.size());
}

Here as the name suggests I check if the workouts I get back are not equal to 0. In this test I test the unit allWorkoutsByUser in the workoutService. 

### <a name="_toc124599916"></a>Results

When running the tests local it gives all green. 

### <a name="_toc124599917"></a>CI/CD
I automated this process in my CI. When building the application with ./gradlew build the application runs these tests automatically. 


## <a name="_toc124599918"></a>integration testing
### <a name="_toc124599919"></a>What did I use? 
For integration tests I again used Junit with @QuarkusTest. 

### <a name="_toc124599920"></a>How did I use it?
@Test
@Transactional
@TestSecurity(user = "carlovankessel@yahoo.nl", roles = "user")
public void When\_Post\_Workout\_Succeeds() {
`    `*given*()
.header(HttpHeaders.*CONTENT\_TYPE*, MediaType.*APPLICATION\_JSON*)
.header(HttpHeaders.*ACCEPT*, MediaType.*APPLICATION\_JSON*)
.when().post("/" + "Legs" + "/" + "carlovankessel@yahoo.nl")
.then()
.statusCode(201);

`    `*given*()
.when().get("/user/carlovankessel@yahoo.nl")
.then()
.statusCode(200)
.body("$.size()", *is*(3));
}

The first givin adds a workout to the user. It checks this automatically because of the status code 201 which means it is created. Then with the second given it check if it is actually created by checking if the user has 3 workouts.

### <a name="_toc124599921"></a>Results
The results are the same with the unit tests. 

### <a name="_toc124599922"></a>CI/CD
The CI is the exact same as the unit tests. 

## <a name="_toc124599923"></a>Security testing
To make sure the user who tries to access the backend endpoint needs to be verified by Keycloak. This is done with the @TestSecurity I give the user email. Then it will check automatically if it is valid. 

@Test
@TestSecurity(user = "admin@yahoo.nl", roles = "user")
public void When\_Get\_Exercises\_By\_Workout\_And\_User\_Fails\_Because\_TokenEmail\_And\_Email\_Arent\_From\_The\_Same\_User() {
`    `// Arrange
`    `String email = "carlovankessel@yahoo.nl";
`    `UUID workoutId = UUID.*fromString*("32b5646c-cecb-4518-ae17-bcd296990db7");

`    `// Assert
`    `Assertions.*assertThrows*(IllegalArgumentException.class, () -> {
`        `exerciseService.findExercisesByWorkoutId(workoutId, email);
`    `});
}

As you can see here when trying a different email than the one in the token it fails. 

## <a name="_toc124599924"></a>Static code analysis
### <a name="_toc124599925"></a>What did I use?
For the backend I used the same as the frontend SonarCloud. 

### <a name="_toc124599926"></a>How did I use it?
In Quarkus to add SonarCloud you need to add the credentials in the build.gradle file: 

sonarqube **{**
`    `properties **{**
`        `property "sonar.projectKey", "WorkoutTracking\_Backend"
`        `property "sonar.projectName", "Backend"
`        `property "sonar.organization", "workouttracking"
`        `property "sonar.host.url", "https://sonarcloud.io"
`    `**}
}**

### <a name="_toc124599927"></a>Results

Here you can see the results of the scan. For my backend the code coverage does work. I used the JacocoTestReport to make a report that SonarCloud can read to get the coverage. 

### <a name="_toc124599928"></a>CI/CD
I automated this scan in my CI, which gives me the exact same thing as the frontend. 


# <a name="_toc124599929"></a>Test files
## <a name="_toc124599930"></a>Frontend
1. [Unit test files](https://github.com/WorkoutTracking/Frontend/tree/main/tests/unit)
1. [End-to-end files](https://github.com/WorkoutTracking/Frontend/tree/main/cypress/e2e)
1. [Load test file](https://github.com/WorkoutTracking/Frontend/blob/main/load-testing.yml)

## <a name="_toc124599931"></a>Backend
1. [Unit test files](https://github.com/WorkoutTracking/Backend/tree/main/src/test/java/com/wt/service)
1. [Integration test files](https://github.com/WorkoutTracking/Backend/tree/main/src/test/java/com/wt/resource)
1. [Security test files](https://github.com/WorkoutTracking/Backend/tree/main/src/test/java/com/wt/resource)


# <a name="_toc124599932"></a>Sources 
<https://www.youtube.com/watch?v=QIDhzBg5eWY> 
