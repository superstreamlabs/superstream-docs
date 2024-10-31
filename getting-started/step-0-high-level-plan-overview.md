# Step 0: High-level plan overview

Superstream high-level implementation guide:



<details>

<summary>Fully managed</summary>

### Step 1: Login

Upon receiving an email from Superstream, head over to the [SSM Console](https://app.superstream.ai) and log in

### Step 2: Connect

Connect at least one Kafka cluster through an API key and/or an Engine

### _<mark style="color:purple;">Optional</mark>_. Step 3: Clients

Replace client packages with the equivalent SSM package.

### Step 4: Sanity checks

* Engine deployment is healthy and monitored
* Clusters are connected properly
* Optimizations are being identified and highlighted
* _<mark style="color:purple;">Optional</mark>._ Client properties are retrieved and can be updated

</details>

<details>

<summary>Hybrid: local engine</summary>

### Step 1: Fill out the "Environment Readiness Checklist"

Please complete the [following checklist](https://docs.google.com/spreadsheets/d/1z-IRt6jBhMpL-T9XhL0k1hoPHgAZnlSoPh0ay2ymses/edit?usp=sharing) to ensure your environment is ready for deployment. Once finished, please share it with your success engineer.

### Step 2: Deploy the SSM engine

Deploy at least one SSM engine following the [step-by-step](step-2-engine-deployment.md) guide provided on the documentation site.

### Step 3: Head over to the SSM Console

Superstream management takes place through the [SSM console](https://app.superstream.ai).

### Step 4: Connect at least one Kafka cluster through an API key and/or an Engine

### Step 5: Client connectivity

Replace client packages with the equivalent SSM package.

### Step 6: Sanity checks

We make sure the entire deployment is resilient and works as expected:

* Engine deployment is healthy and monitored
* Clusters are connected properly
* Optimizations are being identified and highlighted
* Client properties are retrieved and can be updated

</details>
