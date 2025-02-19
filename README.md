# ReactFirebase

---

### 1. **Firebase Setup (`firebase.js`)**

```javascript
// Import Firebase modules
import { initializeApp } from "firebase/app"; // To initialize Firebase
import { getAuth } from "firebase/auth"; // To handle authentication

// Firebase configuration object (replace with your Firebase project's config)
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID",
};

// Initialize Firebase with the configuration
const app = initializeApp(firebaseConfig);

// Export the authentication module to use in other components
export const auth = getAuth(app);
```

---

### 2. **Signup Component (`Signup.js`)**

```javascript
import React, { useState } from "react"; // Import React and useState hook
import { createUserWithEmailAndPassword } from "firebase/auth"; // Firebase function to create a user
import { auth } from "./firebase"; // Import the auth object from Firebase setup
import { useNavigate } from "react-router-dom"; // For navigation after signup

const Signup = () => {
  // State to store email and password
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  // useNavigate hook to programmatically navigate to other pages
  const navigate = useNavigate();

  // Function to handle signup
  const handleSignup = async (e) => {
    e.preventDefault(); // Prevent default form submission behavior
    try {
      // Create a new user with email and password using Firebase
      await createUserWithEmailAndPassword(auth, email, password);
      alert("User created successfully!"); // Show success message
      navigate("/login"); // Redirect to the login page after successful signup
    } catch (error) {
      alert(error.message); // Show error message if signup fails
    }
  };

  return (
    <div>
      <h2>Signup</h2>
      {/* Signup form */}
      <form onSubmit={handleSignup}>
        <input
          type="email"
          placeholder="Email"
          value={email}
          onChange={(e) => setEmail(e.target.value)} // Update email state on input change
        />
        <input
          type="password"
          placeholder="Password"
          value={password}
          onChange={(e) => setPassword(e.target.value)} // Update password state on input change
        />
        <button type="submit">Sign Up</button> {/* Submit button */}
      </form>
    </div>
  );
};

export default Signup; // Export the Signup component
```

---

### 3. **Login Component (`Login.js`)**

```javascript
import React, { useState } from "react"; // Import React and useState hook
import { signInWithEmailAndPassword } from "firebase/auth"; // Firebase function to log in a user
import { auth } from "./firebase"; // Import the auth object from Firebase setup
import { useNavigate } from "react-router-dom"; // For navigation after login

const Login = () => {
  // State to store email and password
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  // useNavigate hook to programmatically navigate to other pages
  const navigate = useNavigate();

  // Function to handle login
  const handleLogin = async (e) => {
    e.preventDefault(); // Prevent default form submission behavior
    try {
      // Log in the user with email and password using Firebase
      await signInWithEmailAndPassword(auth, email, password);
      alert("Logged in successfully!"); // Show success message
      navigate("/"); // Redirect to the home page after successful login
    } catch (error) {
      alert(error.message); // Show error message if login fails
    }
  };

  return (
    <div>
      <h2>Login</h2>
      {/* Login form */}
      <form onSubmit={handleLogin}>
        <input
          type="email"
          placeholder="Email"
          value={email}
          onChange={(e) => setEmail(e.target.value)} // Update email state on input change
        />
        <input
          type="password"
          placeholder="Password"
          value={password}
          onChange={(e) => setPassword(e.target.value)} // Update password state on input change
        />
        <button type="submit">Login</button> {/* Submit button */}
      </form>
    </div>
  );
};

export default Login; // Export the Login component
```

---

### 4. **Home Component (`Home.js`)**

```javascript
import React from "react"; // Import React
import { auth } from "./firebase"; // Import the auth object from Firebase setup
import { signOut } from "firebase/auth"; // Firebase function to log out a user
import { useNavigate } from "react-router-dom"; // For navigation after logout

const Home = () => {
  // useNavigate hook to programmatically navigate to other pages
  const navigate = useNavigate();

  // Function to handle logout
  const handleLogout = async () => {
    try {
      await signOut(auth); // Sign out the user using Firebase
      navigate("/login"); // Redirect to the login page after logout
    } catch (error) {
      alert(error.message); // Show error message if logout fails
    }
  };

  return (
    <div>
      <h2>Welcome to the Home Page!</h2>
      {/* Logout button */}
      <button onClick={handleLogout}>Logout</button>
    </div>
  );
};

export default Home; // Export the Home component
```

---

### 5. **Routing Setup (`App.js`)**

```javascript
import React from "react"; // Import React
import { BrowserRouter as Router, Routes, Route } from "react-router-dom"; // For routing
import Login from "./Login"; // Import Login component
import Signup from "./Signup"; // Import Signup component
import Home from "./Home"; // Import Home component

const App = () => {
  return (
    <Router>
      {/* Define routes */}
      <Routes>
        <Route path="/login" element={<Login />} /> {/* Route for Login page */}
        <Route path="/signup" element={<Signup />} /> {/* Route for Signup page */}
        <Route path="/" element={<Home />} /> {/* Route for Home page */}
      </Routes>
    </Router>
  );
};

export default App; // Export the App component
```

---

### 6. **Protected Route (Optional)**

```javascript
import { Navigate, Outlet } from "react-router-dom"; // For protected routing
import { auth } from "./firebase"; // Import the auth object from Firebase setup

const ProtectedRoute = () => {
  const user = auth.currentUser; // Get the current authenticated user
  return user ? <Outlet /> : <Navigate to="/login" />; // If user is logged in, render the child routes; otherwise, redirect to login
};

export default ProtectedRoute; // Export the ProtectedRoute component
```

Update `App.js` to use the `ProtectedRoute`:

```javascript
<Route element={<ProtectedRoute />}>
  <Route path="/" element={<Home />} /> {/* Protect the Home route */}
</Route>
```

---

### Summary of Comments
- **Firebase Setup**: Initializes Firebase and exports the `auth` object.
- **Signup Component**: Handles user registration using Firebase.
- **Login Component**: Handles user authentication using Firebase.
- **Home Component**: Displays a welcome message and a logout button.
- **Routing**: Manages navigation between login, signup, and home pages.
- **Protected Route**: Ensures only authenticated users can access certain routes.
