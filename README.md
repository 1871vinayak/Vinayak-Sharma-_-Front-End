# Frontend Engineer Assignment
To answer the following quries based on the code provided.
##### React Component Code Review


[![Respected-Sir/Ma'am](https://img.shields.io/badge/Respected-Sir%2FMa'am-brightgreen?style=for-the-badge&logo=castro)]()

The Solution for the given assignment is as follows:
  
------------

### Q1. Explain what the simple `List` component does.
```
To be exact, while rendering a list of objects on our application, we use the List Component. For instance, a list of Candidates or a list of products, etc.
The map() function is typically used to display data from a list or array on the DOM or in another component.
```
  
------------

### Q2. What problems / warnings are there with code?
- Warning/Error (1):

	[![Error](https://img.shields.io/badge/Error-red)]() 👇
  
  `_propTypes.default.shapeOf is not a function`
	```javascript
	WrappedListComponent.propTypes = {
	  items: PropTypes.array(PropTypes.shapeOf({
	    text: PropTypes.string.isRequired,
	  })),
	};
	```
	
	[![Fixed](https://img.shields.io/badge/Fixed-success)]() 👇
  ```javascript
	WrappedListComponent.propTypes = {
	  items: PropTypes.arrayOf(PropTypes.shape({
	    text: PropTypes.string.isRequired,
	  })),
	};
  ```
- Warning/Error (2):

	[![Error](https://img.shields.io/badge/Error-red)]() 👇
	```javascript
	onClick={onClickHandler(index)}
	```
	`onClick` Function Should be passed rather the calling it directly.
  
  [![Fixed](https://img.shields.io/badge/Fixed-success)]() 👇
  ```javascript
	onClick={() => onClickHandler(index)}
  ```
- Warning/Error (3):

	[![Fix](https://img.shields.io/badge/Fix-red)]() 👇
	```javascript
	WrappedSingleListItem.propTypes = {
	  index: PropTypes.number,
	  isSelected: PropTypes.bool,
	  onClickHandler: PropTypes.func.isRequired,
	  text: PropTypes.string.isRequired,
	};
	```
	Although it is not a problem,
	the `index: PropTypes.number` and `isSelected: PropTypes.bool`
	within `WrappedSingleListItem.propTypes` 
	do not have a necessary flag. As these two parameters are essential for the
	code's proper operation, it would be a good idea to add a required flag so that
	programmers do not forget to pass them.

	[![Fixed](https://img.shields.io/badge/Fixed-success)]() 👇
  ```javascript
	WrappedSingleListItem.propTypes = {
	  index: PropTypes.number.isRequired,
	  isSelected: PropTypes.bool.isRequired,
	  onClickHandler: PropTypes.func.isRequired,
	  text: PropTypes.string.isRequired,
	};
  ```
- Warning/Error (4):

	[![Error](https://img.shields.io/badge/Error-red)]() 👇
	```javascript
	const [setSelectedIndex, selectedIndex] = useState();
	```
	`useState()` is not utilised correctly in the `WrappedListComponent` simply because the
	variable name is used before the function name.

  [![Fixed](https://img.shields.io/badge/Fixed-success)]() 👇
  ```javascript
	const [selectedIndex, setSelectedIndex] = useState();
  ```
- Warning/Error (5):

	[![Error](https://img.shields.io/badge/Error-red)]() 👇
	```javascript
	const [setSelectedIndex, selectedIndex] = useState();
	
	  useEffect(() => {
	    setSelectedIndex(null);
	  }, [items]);
	```
	Since `selectedIndex` is an integer, the value `null` supplied to it is incorrect because it
	should be assigned with a numeric value like `-1` at the beginning to indicate that
	nothing is selected.

  [![Fixed](https://img.shields.io/badge/Fixed-success)]() 👇
  ```javascript
	const [setSelectedIndex, selectedIndex] = useState(-1);
	
	  useEffect(() => {
	    setSelectedIndex(-1);
	  }, [items]);
  ```
- Warning/Error (6):

	[![Error](https://img.shields.io/badge/Error-red)]() 👇
	```javascript
	<SingleListItem
	          onClickHandler={() => handleClick(index)}
	          text={item.text}
	          index={index}
	          isSelected={selectedIndex}
	 />
	```
	The selected index should correspond to the index, and key property has not yet
	been incorporated.

	[![Fixed](https://img.shields.io/badge/Fixed-success)]() 👇
  ```javascript
	<SingleListItem
	          key={index}
	          onClickHandler={() => handleClick(index)}
	          text={item.text}
	          index={index}
	          isSelected={selectedIndex}
	 />
  ```
- Warning/Error (7):

	[![Error](https://img.shields.io/badge/Error-red)]() 👇
	```javascript
	WrappedListComponent.defaultProps = {
	  items: null,
	};
	```
	Inside `WrappedListComponent.defaultProps` items cannot be `null` Therefore, the
	following items should be added:

	[![Fixed](https://img.shields.io/badge/Fixed-success)]() 👇
  ```javascript
	WrappedListComponent.defaultProps = {
	  items:  [
	    {text:"1"},
	    {text:"2"},
	    ],
	};
  ```
  
------------

### Q3. Please fix, optimize, and/or modify the component as much as you think is necessary.

```javascript
import React, { useState, useEffect, memo } from 'react';
import PropTypes from 'prop-types';

// Single List Item
const WrappedSingleListItem = ({
  index,
  isSelected,
  onClickHandler,
  text,
}) => {
  return (
    <li
      style={{ backgroundColor: isSelected ? 'green' : 'red'}}
      onClick={() => onClickHandler(index)} //Warning/Error (2)
    >
      {text}
    </li>
  );
};
//Warning/Error (3)
WrappedSingleListItem.propTypes = {
  index: PropTypes.number.isRequired,
  isSelected: PropTypes.bool.isRequired,
  onClickHandler: PropTypes.func.isRequired,
  text: PropTypes.string.isRequired,
};

const SingleListItem = memo(WrappedSingleListItem);

// List Component
const WrappedListComponent = ({
  items,
}) => {
  //Warning/Error (4) & Warning/Error (5)
  const [selectedIndex, setSelectedIndex] = useState(-1);
  
  useEffect(() => {
    setSelectedIndex(-1);
  }, [items]);

  const handleClick = index => {
    setSelectedIndex(index);
  };

  return (
    <ul style={{ textAlign: 'left' }}>
      {items.map((item, index) => (
        //Warning/Error (6)
          <SingleListItem
          key={index}
          onClickHandler={() => handleClick(index)}
          text={item.text}
          index={index}
          isSelected={selectedIndex}
          />
      ))}
    </ul>
  )
};
// Warning/Error (1)
WrappedListComponent.propTypes = {
  items: PropTypes.arrayOf(PropTypes.shape({
    text: PropTypes.string.isRequired,
  })),
};

//Warning/Error (7)
WrappedListComponent.defaultProps = {
  items:  [
    {text:"1"},
    {text:"2"},
    ],
};

const List = memo(WrappedListComponent);

export default List;

```
