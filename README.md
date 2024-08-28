To build a rapid prototype of the front-end for an Electronic Health Record (EHR) system that tracks the quantity and availability of medicines, we'll use React. This prototype will demonstrate the basic functionality of viewing and managing the medicine inventory.

### **Objective:**

The objective of this prototype is to create a simple, interactive user interface that allows users to:

1. **View a list of medicines:** Display each medicine's name, quantity, and availability status.
2. **Update medicine information:** Allow users to update the quantity of a specific medicine.
3. **Add or remove medicines:** Provide functionality to add new medicines to the inventory or remove them.

### **Steps to Build the Prototype:**

1. **Set Up the React Environment:**
   - Use `create-react-app` to set up a new React project quickly.

2. **Create Components:**
   - **MedicineList Component:** To display the list of medicines.
   - **MedicineItem Component:** To represent each medicine in the list.
   - **AddMedicine Component:** To provide a form for adding new medicines.

3. **State Management:**
   - Use React's `useState` hook to manage the state of the medicine list.
   - Implement functions to handle adding, updating, and removing medicines.

4. **Styling:**
   - Use basic CSS for styling the components to ensure the interface is user-friendly.

### **Prototype Code:**

Below is the code structure for the prototype:

1. **Set Up Project:**
   - First, set up a new React project by running:

     ```bash
     npx create-react-app ehr-medicine-tracker
     cd ehr-medicine-tracker
     ```

2. **Build the Components:**

   - **App.js:** Main component managing state and rendering other components.

     ```jsx
     import React, { useState } from 'react';
     import MedicineList from './components/MedicineList';
     import AddMedicine from './components/AddMedicine';
     import './App.css';

     const App = () => {
       const [medicines, setMedicines] = useState([
         { id: 1, name: 'Paracetamol', quantity: 100 },
         { id: 2, name: 'Ibuprofen', quantity: 50 },
       ]);

       const addMedicine = (medicine) => {
         setMedicines([...medicines, { ...medicine, id: medicines.length + 1 }]);
       };

       const updateMedicineQuantity = (id, quantity) => {
         const updatedMedicines = medicines.map((medicine) =>
           medicine.id === id ? { ...medicine, quantity } : medicine
         );
         setMedicines(updatedMedicines);
       };

       return (
         <div className="App">
           <h1>Electronic Health Record System - Medicine Tracker</h1>
           <AddMedicine addMedicine={addMedicine} />
           <MedicineList medicines={medicines} updateMedicineQuantity={updateMedicineQuantity} />
         </div>
       );
     };

     export default App;
     ```

   - **MedicineList.js:** Component to display the list of medicines.

     ```jsx
     import React from 'react';
     import MedicineItem from './MedicineItem';

     const MedicineList = ({ medicines, updateMedicineQuantity }) => {
       return (
         <div>
           <h2>Medicine Inventory</h2>
           <ul>
             {medicines.map((medicine) => (
               <MedicineItem
                 key={medicine.id}
                 medicine={medicine}
                 updateMedicineQuantity={updateMedicineQuantity}
               />
             ))}
           </ul>
         </div>
       );
     };

     export default MedicineList;
     ```

   - **MedicineItem.js:** Component representing each medicine item.

     ```jsx
     import React, { useState } from 'react';

     const MedicineItem = ({ medicine, updateMedicineQuantity }) => {
       const [quantity, setQuantity] = useState(medicine.quantity);

       const handleChange = (e) => {
         setQuantity(e.target.value);
       };

       const handleUpdate = () => {
         updateMedicineQuantity(medicine.id, quantity);
       };

       return (
         <li>
           {medicine.name} - Quantity: {medicine.quantity}
           <input type="number" value={quantity} onChange={handleChange} />
           <button onClick={handleUpdate}>Update</button>
         </li>
       );
     };

     export default MedicineItem;
     ```

   - **AddMedicine.js:** Component for adding new medicines.

     ```jsx
     import React, { useState } from 'react';

     const AddMedicine = ({ addMedicine }) => {
       const [name, setName] = useState('');
       const [quantity, setQuantity] = useState(0);

       const handleSubmit = (e) => {
         e.preventDefault();
         addMedicine({ name, quantity });
         setName('');
         setQuantity(0);
       };

       return (
         <form onSubmit={handleSubmit}>
           <h2>Add New Medicine</h2>
           <input
             type="text"
             placeholder="Medicine Name"
             value={name}
             onChange={(e) => setName(e.target.value)}
           />
           <input
             type="number"
             placeholder="Quantity"
             value={quantity}
             onChange={(e) => setQuantity(Number(e.target.value))}
           />
           <button type="submit">Add Medicine</button>
         </form>
       );
     };

     export default AddMedicine;
     ```

3. **Styling (App.css):** Basic styles to make the interface user-friendly.

   ```css
   .App {
     font-family: Arial, sans-serif;
     text-align: center;
     margin: 20px;
   }

   h1, h2 {
     color: #333;
   }

   ul {
     list-style-type: none;
     padding: 0;
   }

   li {
     margin: 10px 0;
   }

   input {
     margin: 0 5px;
   }
   ```
