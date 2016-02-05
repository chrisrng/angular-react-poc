# angular-react-poc
POC for using React Components in Angular.

## How to measure performance in angular-react-poc

Use `window.performance` from the [User Timing API](https://www.w3.org/TR/hr-time/) to have accurate time measurement in milliseconds.

Trigger a start mark on the `scope.load` function call.

Trigger an end mark on the `$timeout` call to mark the end of an Angular render and hook into `componentDidMount` to mark the end of a React render.

### Setup:
- When an HTML is launched, it automatically does a load of 100 rows of pascal's triangle.
- Everytime you click on a "Load X" button, it does a performance mark of loading a new pascal's triangle on top of the old one with X rows.
- Thus if repeatedly clicking on the same "Load X" button, the time it takes to render is significantly less (because of the virtual dom or using track by)

## Results

Averaging at around `39.07%` increase in performance over 10, 50, and 100 rows of pascal's triangle.

### Angular performance:

![311.315ms for 100 rows](https://raw.github.com/chrisrng/angular-react-poc/master/images/angular-only.png)

### Angular + React performance:

![81.435ms for 100 rows](https://raw.github.com/chrisrng/angular-react-poc/master/images/angular-react.png)
