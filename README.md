# Teenjobsconnect1
import React, { useState, useEffect } from 'react';

const App = () => {
  const [theme] = useState({
    primary: 'purple-600',
    background: 'black',
    text: 'white'
  });

  return (
    <div className={`bg-${theme.background} min-h-screen text-${theme.text}`}>
      <header className={`bg-${theme.primary} p-4`}>
        <h1 className="text-2xl font-bold">TeenJobsConnect</h1>
      </header>
      
      <main className="container mx-auto p-4">
        <JobBoard />
        <MapView />
        <PaymentSystem />
      </main>
    </div>
  );
};

export default App;
const JobBoard = () => {
  const [jobs, setJobs] = useState([]);
  const [filters, setFilters] = useState({
    type: 'all',
    distance: 10,
    payment: 'any'
  });

  return (
    <div className="bg-black p-6 rounded-xl">
      <h2 className="text-2xl text-purple-300 mb-6">Available Jobs</h2>
      
      {/* Job Filters */}
      <div className="grid grid-cols-3 gap-4 mb-6">
        <select className="bg-gray-900 border border-purple-500 rounded p-2">
          <option value="all">All Jobs</option>
          <option value="lawn">Lawn Care</option>
          <option value="misc">Misc Jobs</option>
        </select>
        
        <select className="bg-gray-900 border border-purple-500 rounded p-2">
          <option value="any">Any Payment</option>
          <option value="20+">$20+</option>
          <option value="50+">$50+</option>
        </select>
        
        <select className="bg-gray-900 border border-purple-500 rounded p-2">
          <option value="10">10 miles</option>
          <option value="20">20 miles</option>
          <option value="30">30 miles</option>
        </select>
      </div>

      {/* Job Listings */}
      <div className="space-y-4">
        {jobs.map(job => (
          <JobCard key={job.id} job={job} />
        ))}
      </div>
    </div>
  );
};

export default JobBoard;
const PaymentSystem = () => {
  const [transactions, setTransactions] = useState([]);
  const platformFee = 0.05; // 5%

  const processPayment = (payment) => {
    const transaction = {
      id: generateUniqueId(),
      amount: payment.amount,
      fee: payment.amount * platformFee,
      teenPayout: payment.amount * (1 - platformFee),
      status: 'processing',
      timestamp: Date.now()
    };

    return transaction;
  };

  return (
    <div className="bg-black p-6 rounded-xl">
      <h2 className="text-2xl text-purple-300 mb-6">Payment Processing</h2>
      
      {/* Payment Form */}
      <div className="space-y-4 mb-6">
        <input 
          type="text" 
          placeholder="Card Number"
          className="w-full bg-gray-900 border border-purple-500 rounded p-2"
        />
        <div className="grid grid-cols-2 gap-4">
          <input 
            type="text" 
            placeholder="MM/YY"
            className="bg-gray-900 border border-purple-500 rounded p-2"
          />
          <input 
            type="text" 
            placeholder="CVV"
            className="bg-gray-900 border border-purple-500 rounded p-2"
          />
        </div>
        <button className="w-full bg-purple-600 text-white py-2 rounded">
          Process Payment
        </button>
      </div>

      {/* Transaction History */}
      <div className="space-y-2">
        {transactions.map(tx => (
          <div key={tx.id} className="bg-gray-900 p-4 rounded">
            <div className="flex justify-between">
              <span>${tx.amount}</span>
              <span>{tx.status}</span>
            </div>
          </div>
        ))}
      </div>
    </div>
  );
};

export default PaymentSystem;
const MapSystem = () => {
  const [coordinates, setCoordinates] = useState({
    latitude: null,
    longitude: null
  });

  const calculateDistance = (point1, point2) => {
    const R = 6371;
    const dLat = toRad(point2.lat - point1.lat);
    const dLon = toRad(point2.lon - point1.lon);
    return Math.round(R * Math.sqrt(dLat * dLat + dLon * dLon));
  };

  return (
    <div className="bg-black p-6 rounded-xl">
      <h2 className="text-2xl text-purple-300 mb-6">Job Locations</h2>
      
      <div className="h-[400px] bg-gray-900 rounded-lg relative">
        {/* Map Display */}
        <div className="absolute inset-0">
          {activeJobs.map(job => (
            <JobMarker 
              key={job.id}
              position={job.location}
              onClick={() => selectJob(job)}
            />
          ))}
        </div>

        {/* Distance Controls */}
        <div className="absolute bottom-4 left-4 bg-black p-2 rounded">
          <select className="bg-gray-900 border border-purple-500 rounded p-2">
            <option>5 miles</option>
            <option>10 miles</option>
            <option>20 miles</option>
          </select>
        </div>
      </div>
    </div>
  );
};

export default MapSystem;
const AdminDashboard = () => {
  const [stats, setStats] = useState({
    totalUsers: 0,
    activeJobs: 0,
    revenue: 0
  });

  return (
    <div className="bg-black p-6 rounded-xl">
      <h2 className="text-2xl text-purple-300 mb-6">Admin Control Panel</h2>
      
      {/* Stats Overview */}
      <div className="grid grid-cols-3 gap-4 mb-6">
        <div className="bg-gray-900 p-4 rounded">
          <div className="text-2xl text-purple-300">{stats.totalUsers}</div>
          <div className="text-sm">Total Users</div>
        </div>
        <div className="bg-gray-900 p-4 rounded">
          <div className="text-2xl text-purple-300">{stats.activeJobs}</div>
          <div className="text-sm">Active Jobs</div>
        </div>
        <div className="bg-gray-900 p-4 rounded">
          <div className="text-2xl text-purple-300">${stats.revenue}</div>
          <div className="text-sm">Platform Revenue</div>
        </div>
      </div>

      {/* User Management */}
      <div className="space-y-4">
        {users.map(user => (
          <div key={user.id} className="bg-gray-900 p-4 rounded flex justify-between">
            <div>
              <div className="text-white">{user.name}</div>
              <div className="text-sm text-purple-300">{user.email}</div>
            </div>
            <button className="bg-red-600 px-4 py-2 rounded">
              Delete Account
            </button>
          </div>
        ))}
      </div>
    </div>
  );
};

export default AdminDashboard;
const Database = {
  users: [],
  jobs: [],
  transactions: [],
  ratings: [],

  // User Operations
  addUser: (userData) => {
    const newUser = {
      id: generateUniqueId(),
      ...userData,
      createdAt: Date.now()
    };
    Database.users.push(newUser);
    return newUser;
  },

  // Job Operations
  createJob: (jobData) => {
    const newJob = {
      id: generateUniqueId(),
      status: 'active',
      ...jobData,
      postedAt: Date.now()
    };
    Database.jobs.push(newJob);
    return newJob;
  },

  // Transaction Operations
  processTransaction: (transactionData) => {
    const newTransaction = {
      id: generateUniqueId(),
      ...transactionData,
      timestamp: Date.now()
    };
    Database.transactions.push(newTransaction);
    return newTransaction;
  }
};

export default Database;
const Authentication = {
  // User Registration
  registerUser: (userData) => {
    const hashedPassword = hashPassword(userData.password);
    const newUser = {
      id: generateUniqueId(),
      ...userData,
      password: hashedPassword,
      verified: false,
      createdAt: Date.now()
    };
    return newUser;
  },

  // Login System
  login: (email, password) => {
    const user = Database.users.find(u => u.email === email);
    if (user && verifyPassword(password, user.password)) {
      const session = createSession(user.id);
      return { user, token: session.token };
    }
    return null;
  },

  // Session Management
  createSession: (userId) => {
    const token = generateSecureToken();
    const session = {
      userId,
      token,
      createdAt: Date.now(),
      expiresAt: Date.now() + (24 * 60 * 60 * 1000)
    };
    return session;
  }
};

export default Authentication;
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TeenJobsConnect</title>
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css" rel="stylesheet">
</head>
<body>
    <div id="root"></div>
    <script src="../src/index.js"></script>
</body>
</html>
