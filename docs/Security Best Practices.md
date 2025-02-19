# Cerify Backend Architecture & Deployment Guide

Cerify is an advanced blockchain-based platform for analyzing, verifying, and certifying smart contracts. This guide explains our complete microservices architecture, including the Firestore-backed authentication and database service, and the AWS-based model inference service. The architecture is designed to be modular, scalable, and secure, enabling seamless integration between backend services and the frontend.

---

## Table of Contents

- [Overview](#overview)
- [Architecture Overview](#architecture-overview)
- [Firestore Setup & Authentication Service](#firestore-setup--authentication-service)
  - [Prerequisites](#prerequisites)
  - [Configuration (firebaseConfig.js)](#configuration-firebaseconfigjs)
  - [User Signup & Login API (server.js)](#user-signup--login-api-serversjs)
- [AWS Model Inference Service](#aws-model-inference-service)
  - [AWS Lambda Function Setup](#aws-lambda-function-setup)
  - [Lambda Code Example (lambda_function.js)](#lambda-code-example-lambda_functionjs)
  - [Deployment Instructions](#deployment-instructions)
- [Frontend Integration](#frontend-integration)
  - [Fetching Analysis Results (fetchResults.js)](#fetching-analysis-results-fetchresultsjs)
- [Microservices Communication Flow](#microservices-communication-flow)
- [Additional Considerations](#additional-considerations)
- [Conclusion](#conclusion)

---

## Overview

Cerify's backend is built on a microservices architecture that separates user authentication and data management from the computationally intensive smart contract analysis. This design ensures that each service can be scaled independently while maintaining secure and reliable data exchange.

---

## Architecture Overview

1. **Authentication & Database Service (Firestore):**  
   - Manages user signup, login, and stores user profiles, smart contract submissions, and analysis results.
   - Utilizes Firebase Firestore, a scalable NoSQL document database.

2. **Model Inference Service (AWS):**  
   - Hosts the smart contract security model as a serverless AWS Lambda function.
   - Processes submitted contracts, generates security analysis, and saves the results back to Firestore.
   - Exposes a REST API via AWS API Gateway.

3. **Frontend Integration:**  
   - Users interact with the frontend to register, submit contracts, and view analysis reports.
   - The frontend communicates with both backend services to perform authentication and fetch results.

---

## Firestore Setup & Authentication Service

### Prerequisites
- A [Firebase project](https://console.firebase.google.com/) with Firestore enabled in Native mode.
- Node.js installed for running the backend server.
- Firebase SDK installed in your Node.js project.

### Configuration (firebaseConfig.js)

Create a file named `firebaseConfig.js` to initialize Firebase and Firestore:

```javascript
// firebaseConfig.js
const firebase = require("firebase/app");
require("firebase/auth");
require("firebase/firestore");

const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "yourproject.firebaseapp.com",
  projectId: "yourproject-id",
  storageBucket: "yourproject.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID",
};

firebase.initializeApp(firebaseConfig);

const auth = firebase.auth();
const firestore = firebase.firestore();

module.exports = { firebase, auth, firestore };


```
User Login and Signup
// server.js
const express = require("express");
const { auth, firestore } = require("./firebaseConfig");
const app = express();

app.use(express.json());

// Signup endpoint
app.post("/signup", async (req, res) => {
  try {
    const { email, password, name } = req.body;
    const userCredential = await auth.createUserWithEmailAndPassword(email, password);
    const user = userCredential.user;
    
    // Save additional user information in Firestore
    await firestore.collection("users").doc(user.uid).set({
      name,
      email,
      createdAt: new Date(),
    });
    
    res.status(201).json({ message: "User created successfully", uid: user.uid });
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

// Login endpoint
app.post("/login", async (req, res) => {
  try {
    const { email, password } = req.body;
    const userCredential = await auth.signInWithEmailAndPassword(email, password);
    const user = userCredential.user;
    res.status(200).json({ message: "Login successful", uid: user.uid });
  } catch (error) {
    res.status(400).json({ error: error.message });
  }
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Auth service listening on port ${PORT}`);
});  
