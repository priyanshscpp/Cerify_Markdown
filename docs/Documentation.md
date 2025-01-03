
# Cerify Systems: Frontend

## Overview
Cerify is a platform designed to enable users to upload and manage smart contracts with automated verification, scoring, and certification. The project integrates blockchain technology with modern backend and frontend frameworks to provide a seamless user experience while ensuring secure and efficient handling of smart contracts. This document provides an in-depth overview of the architecture, starting with the frontend and then the backend, and the technologies used in the development of Cerify. It serves as a guide for new developers to understand the codebase and contribute effectively.

---

## Table of Contents

1. **Frontend Overview**
    - User Interface Design
    - Functionalities and Workflow
    - Communication with Backend
2. **Backend Overview**
    - User Management System
    - Smart Contract Handling
    - AI Model Integration
    - Blockchain Interaction
3. **Technologies Used**
4. **Code Examples and API References**
5. **System Design and Architecture**
6. **Future Prospects**

---

## Frontend Overview

### **1. User Interface Design**
The frontend of Cerify is designed with React, ensuring a modular and responsive user experience. The interface is built to guide users seamlessly through the process of uploading, analyzing, and retrieving smart contract reports.

#### Key Components:
- **Authentication Pages:**
  - Login, Signup, and Role-Based Access Control (RBAC) forms.
  - Responsive design for desktop and mobile users.
- **Dashboard:**
  - Displays uploaded contracts, their statuses, and report details.
  - Payment history and user account management.
- **Contract Upload Page:**
  - Interface to upload .sol files (Solidity contracts).
  - Real-time feedback on upload status and AI-generated scores.
- **Report Viewer:**
  - Allows users to download and view detailed reports.

#### Sample React Code: Authentication
```jsx
import React, { useState } from "react";
import { auth } from "../firebaseConfig";

const Login = () => {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleLogin = async () => {
    try {
      await auth.signInWithEmailAndPassword(email, password);
      alert("Login Successful");
    } catch (error) {
      console.error(error);
      alert("Login Failed");
    }
  };

  return (
    <div>
      <h1>Login</h1>
      <input
        type="email"
        placeholder="Email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />
      <input
        type="password"
        placeholder="Password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />
      <button onClick={handleLogin}>Login</button>
    </div>
  );
};

export default Login;
```

---

### **2. Functionalities and Workflow**

#### Pre-Login Functionalities:
- **Smart Contract Upload:**
  - Users can upload a Solidity (.sol) file to receive an initial AI-generated score.
- **Score Display:**
  - Basic contract scores are displayed without login; detailed analysis requires user authentication.

#### Post-Login Functionalities:
- **Dashboard Access:**
  - Displays all uploaded contracts with their statuses and generated reports.
- **Report Generation and Download:**
  - Detailed reports are available after payment and additional processing.

#### Communication with Backend:
Frontend communicates with the backend using REST APIs and WebSocket connections for real-time updates. API requests handle user authentication, contract uploads, and report retrieval.

---

## Backend Overview

### **1. User Management System**
#### Key Responsibilities:
- User authentication and authorization.
- Role-based access control (RBAC).
- Secure session management.

#### Implementation Details:
- **Authentication:** Handled by Firebase Authentication for secure and scalable user credential management.
- **Session Tokens:** Generated and validated on each request to ensure secure access to resources.

#### Code Examples:
```javascript
// Firebase Authentication Example
const firebaseAuth = require("firebase/auth");

const signupUser = async (email, password) => {
  try {
    const userCredential = await firebaseAuth.createUserWithEmailAndPassword(email, password);
    console.log("User Signed Up: ", userCredential.user);
  } catch (error) {
    console.error("Error Signing Up: ", error);
  }
};

const loginUser = async (email, password) => {
  try {
    const userCredential = await firebaseAuth.signInWithEmailAndPassword(email, password);
    console.log("User Logged In: ", userCredential.user);
  } catch (error) {
    console.error("Error Logging In: ", error);
  }
};
```

### **2. Smart Contract Handling**
#### Key Responsibilities:
- Parsing and analyzing uploaded contracts.
- AI-based scoring and vulnerability checks.

#### Implementation Details:
- **Storage:** Contracts are uploaded to Firebase Storage or AWS S3.
- **Processing:** Custom models analyze contracts for security risks, performance, and scalability.

---

## Technologies Used
- **Frontend:** React, Firebase Hosting
- **Backend:** Node.js, Firebase Functions, AWS VMs
- **Database:** Firestore, MongoDB
- **Blockchain Libraries:** Web3.js, Ethers.js

---

## System Design and Architecture

### **High-Level Architecture:**
1. **Frontend:** Handles user interaction and uploads .sol files to Firebase.
2. **Firebase:** Acts as a middleware for authentication and triggers AWS processing.
3. **AWS Backend:** Processes contracts, interacts with the blockchain, and generates reports.

---

## Future Prospects
- **System Scalability:** Design improvements for handling increased user loads.
- **Chatbot Integration:** For user support.
- **Machine Learning Integration:** Enhanced contract analysis.

---

This document serves as a foundational guide for developers to understand and contribute to the Cerify project. For further details, refer to the codebase and associated API documentation.


