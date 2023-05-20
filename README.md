# Employee Directory

This React app displays employee data from a JSON file. Users can search for employees by name and filter them by designation and skills.

## How it works

The app uses the `useState` and `useEffect` hooks from React to manage state and fetch data from the JSON file. The data is fetched from a URL using the `fetch` function and stored in the `data` state variable using the `setData` function.

Users can enter search terms and filters in the input fields at the top of the page. The app uses these values to filter the employee data and display only the employees that match the search term and filters.

The filtering is done using the `Array.prototype.filter()` method. The app checks if each employee's name, designation, and skills match the search term and filters. If an employee matches all of the criteria, they are included in the `filteredEmployees` array.

The app then maps over the `filteredEmployees` array and displays each employee's information in a `div`. The employee's name, designation, skills, and projects are displayed.

## App.js

Here is the code for the `App.js` file:

```javascript
import React, { useState, useEffect } from 'react';
import './App.css';

const App = () => {
 const [data, setData] = useState(null);
 const [searchTerm, setSearchTerm] = useState('');
 const [designationFilter, setDesignationFilter] = useState('');
 const [skillsFilter, setSkillsFilter] = useState('');

 useEffect(() => {
 fetch('https://raw.githubusercontent.com/dixitsoham7/dixitsoham7.github.io/main/index.json')
 .then(response => response.json())
 .then(data => setData(data));
 }, []);

 if (!data) {
 return <p>Loading...</p>;
 }

 const filteredEmployees = data.employees.filter(employee => {
 const matchesName =
 !searchTerm || (employee.name && employee.name.toLowerCase().includes(searchTerm.toLowerCase()));
 const matchesDesignation =
 !designationFilter ||
 (employee.designation &&
 employee.designation.toLowerCase().includes(designationFilter.toLowerCase()));
 const matchesSkills =
 !skillsFilter ||
 (employee.skills && employee.skills.some(skill => skill.toLowerCase().includes(skillsFilter.toLowerCase())));

 return matchesName && matchesDesignation && matchesSkills;
 });

 return (
 <>
 <input
 type="text"
 placeholder="Search by name"
 value={searchTerm}
 onChange={e => setSearchTerm(e.target.value)}
 />
 <input
 type="text"
 placeholder="Filter by designation"
 value={designationFilter}
 onChange={e => setDesignationFilter(e.target.value)}
 />
 <input
 type="text"
 placeholder="Filter by skills"
 value={skillsFilter}
 onChange={e => setSkillsFilter(e.target.value)}
 />
 <div>
 {filteredEmployees.map(employee => (
 <div key={employee.id} className="employee">
 <p><b>Name:</b> {employee.name}</p>
 <p><b>Designation:</b> {employee.designation}</p>
 <p><b>Skills:</b> {employee.skills.join(', ')}</p>
 <p><b>Projects:</b></p>
 {employee.projects && (
 <div className="projects">
 {employee.projects.map(project => (
 <div key={project.name}>
 <p><b>Project Name:</b> {project.name}</p>
 <p><b>Description:</b> {project.description}</p>
 <p><b>Team:</b></p>
 {project.team && (
 <div className="team">
 {project.team.map(member => (
 <div key={member.name}>
 <div className="team-mem">
 <p><b>Name:</b> {member.name}<br/><b>Role:</b> {member.role}</p>
 </div>
 </div>
 ))}
 </div>
 )}
 </div>
 ))}
 </div>
 )}
 </div>
 ))}
 </div>
 </>
 );
};

export default App;
```

## App.css

Here is the code for the `App.css` file:

```css
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 20px;
  background-color: #f4f4f4;
}

input[type='text'] {
  display: inline-block;
  margin-right: 10px;
  padding: 5px;
  border-radius: 5px;
  border: none;
}

.employee {
  border-radius: 5px;
  box-shadow: 0px 0px 5px rgba(0,0,0,0.1);
  padding: 10px;
  margin-top: 10px;
  background-color: white;
}

.employee p {
  margin: 0;
}

.employee p + p {
  margin-top: 5px;
}

.skills,
.projects,
.team {
  margin-left: 20px;
  margin-top: 5px;
  margin-bottom: 5px;
}
.team-mem {
  margin-top: 5px;
  margin-bottom: 5px;
}
```

## Usage

To use this app, clone the repository and install the dependencies:

```
git clone https://github.com/ec-026/fetch.git
cd fetch
npm install
```

Then start the development server:

```
npm start
```

The app will be available at [http://localhost:3000](http://localhost:3000).
