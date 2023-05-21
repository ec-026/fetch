# Employee Directory

This React app displays employee data from a JSON file. Users can search for employees by name and filter them by designation and skills.

## How it works

The app uses the `useState` and `useEffect` hooks from React to manage state and fetch data from the JSON file. The data is fetched from a URL using the `fetch` function and stored in the `data` state variable using the `setData` function.

Users can enter search terms and filters in the input fields at the top of the page. The app uses these values to filter the employee data and display only the employees that match the search term and filters.

The filtering is done using the `Array.prototype.filter()` method. The app checks if each employee's name, designation, and skills match the search term and filters. If an employee matches all of the criteria, they are included in the `filteredEmployees` array.

The app also includes a multi-select dropdown for skills. When you select one or more skills from the dropdown menu, the app filters the list of employees to show only those that have any of the selected skills.
## App.js

Here is the code for the `App.js` file:

```javascript
import React, { useState, useEffect } from 'react';
import './App.css';

const App = () => {
 const [data, setData] = useState(null);
 const [searchTerm, setSearchTerm] = useState('');
 const [designationFilter, setDesignationFilter] = useState('');
 const [selectedSkills, setSelectedSkills] = useState([]);
 const [isDropdownOpen, setIsDropdownOpen] = useState(false);

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
       !selectedSkills.length ||
       (employee.skills && selectedSkills.some(skill => employee.skills.includes(skill)));

   return matchesName && matchesDesignation && matchesSkills;
 });

 const skills = ['SQL', 'JavaScript', 'Python', 'HTML', 'CSS', 'Photoshop', 'Manual Testing', 'Java'];

 return (
   <>
     <div style={{ display: 'flex' }}>
     <input
       type="text"
       placeholder="Search by name"
       value={searchTerm}
       onChange={e => setSearchTerm(e.target.value)}
     />
       <input
         type="text"
         placeholder="Filter by Designation"
         value={designationFilter}
         onChange={e => setDesignationFilter(e.target.value)}
       />
       <div className="skills-dropdown">
         <button onClick={() => setIsDropdownOpen(prevIsDropdownOpen => !prevIsDropdownOpen)}>
         Filter by Skills
         </button>
         {isDropdownOpen && (
           <div className="skills-options">
             {skills.map(skill => (
               <label key={skill}>
                 <input
                   type="checkbox"
                   value={skill}
                   onChange={e => {
                     if (e.target.checked) {
                       setSelectedSkills(prevSelectedSkills => [...prevSelectedSkills, skill]);
                     } else {
                       setSelectedSkills(prevSelectedSkills =>
                         prevSelectedSkills.filter(s => s !== skill)
                       );
                     }
                   }}
                 />
                 {skill}
               </label>
             ))}
           </div>
         )}
       </div>
     </div>
     <div>
       {filteredEmployees.map(employee => (
         <div key={employee.id} className="employee">
           {employee.name ? (
             <p><span>Name:</span> {employee.name}</p>
           ) : (
             <p><span>Name:</span><div className="undefined-value">Undefined</div></p>
           )}
           {employee.designation ? (
             <p><span>Designation:</span> {employee.designation}</p>
           ) : (
             <p><span>Designation:</span><div className="undefined-value">Undefined</div></p>
           )}
           {employee.skills ? (
             <p><span>Skills:</span> {employee.skills.join(', ') || 'None'}</p>
           ) : (
             <p><span>Skills:</span><div className="undefined-value">Undefined</div></p>
           )}
           {employee.projects && employee.projects.length > 0 && (
             <>
               <p><span>Projects:</span></p>
               <div className="projects">
                 {employee.projects.map(project => (
                   <div key={project.name}>
                     {project.name ? (
                       <p><span>Project Name:</span> {project.name}</p>
                     ) : (
                       <p><span>Project Name:</span><div className="undefined-value">undefined</div></p>
                     )}
                     {project.description ? (
                       <p><span>Description:</span> {project.description}</p>
                     ) : (
                       <p><span>Description:</span><div className="undefined-value">Undefined</div></p>
                     )}
                     {project.team && project.team.length > 0 && (
                       <>
                         <p><span>Team:</span></p>
                         <div className="team">
                           {project.team.map(member => (
                             <div key={member.name}>
                               <div className="team-mem">
                                 {member.name ? (
                                   <p><span>Name:</span> {member.name}</p>
                                 ) : (
                                   <p><span>Name:</span><div className="undefined-value">Undefined</div></p>
                                 )}
                                 {member.role ? (
                                   <p><span>Role:</span> {member.role}</p>
                                 ) : (
                                   <p><span>Role:</span><div className="undefined-value">Undefined</div></p>
                                 )}
                               </div>
                             </div>
                           ))}
                         </div>
                       </>
                     )}
                   </div>
                 ))}
               </div>
             </>
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
  font-family: 'Lato', sans-serif;
  margin: 0;
  padding: 20px;
  background-color: #e6f2ff;
}

input[type='text'] {
  display: inline-block;
  margin-right: 10px;
  padding: 10px;
  border-radius: 5px;
  border: none;
  box-shadow: 0px 0px 5px rgba(0,0,0,0.1);
  margin-bottom: 10px;
  transition: all 0.3s ease-in-out;
}

input[type='text']:focus {
  outline: none;
  box-shadow: 0px 0px 5px rgba(0,119,204,0.5);
}

.skills-filter {
 display:flex; 
 flex-wrap:wrap; 
 margin-bottom:10px
}

.skills-filter label {
 display:flex; 
 align-items:center; 
 margin-right:10px; 
 margin-bottom:10px
}

.skills-filter input[type='checkbox'] {
 margin-right:5px
}

.employee {
 border-radius:5px; 
 box-shadow:0px 
0px 
5px rgba(0,0,0,.1); 
padding:20px; 
margin-top:10px; 
background-color:white; 
transition:all .3s ease-in-out; 
border-left:5px solid #0077cc
}

.employee:hover {
box-shadow:0px 
0px 
10px rgba(0,0,0,.3); 
border-left-color:#005999; 
transform:scale(1.02)
}

.employee p {
margin:0
}

.employee p + p {
margin-top:10px
}

.employee span {
display:inline-block; 
width:120px; 
text-align:right; 
background-color:#f2f2f2; 
padding-right:10px; 
font-weight:bold; 
margin-right:10px; 
border-radius:5px
}

.projects,
.team {
margin-left :20px; 
margin-top :10px; 
margin-bottom :10px; 
padding-left :15px; 
border-left :1px solid #ccc
}
.team-mem p + p{
margin-top :5px
}
.team-mem {
margin-top :10px
}

.undefined-value {
 display:inline-block; 
 width :120px; 
 padding-left :10px; 
 font-weight:bold; 
 margin-right :10px; 
 border-radius :5px; 
 background-color:#ff6666; 
 color:white
}
.skills-dropdown {
  position: relative;
}

.skills-dropdown button {
  display: inline-block;
  margin-right: 10px;
  padding: 10px;
  border-radius: 5px;
  border: none;
  box-shadow: 0px 0px 5px rgba(0,0,0,0.1);
  margin-bottom: 10px;
  transition: all 0.3s ease-in-out;
  background-color: #E6FFFD;
}
.skills-dropdown button:hover {
  background-color: #AEE2FF;
}
.skills-dropdown button:focus {
  outline: none;
}

.skills-options {
  position: absolute;
  top: calc(100% + 5px);
  left: 0;
  z-index: 1;
  background-color: white;
  border-radius: 5px;
  box-shadow: 0px 0px 5px rgba(0,0,0,0.1);
  min-width: 200px;
}

.skills-options label {
 display:block; 
 padding :10px; 
 cursor:pointer
}

.skills-options label:hover {
 background-color:#f2f2f2
}

.skills-options input[type='checkbox'] {
 margin-right :5px
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
