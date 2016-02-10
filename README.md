# angular-react-poc
POC for using React Components in Angular.

## Introduction

Sourced from: [http://chrisrng.svbtle.com/angular-react-performance](http://chrisrng.svbtle.com/angular-react-performance).

[Angular](https://angularjs.org/) and performance.

When you do a Google [search](https://www.google.com/search?q=angular+performance) of these two words together, what you will most likely get are [blog](http://www.williambrownstreet.net/blog/2013/07/angularjs-my-solution-to-the-ng-repeat-performance-problem/) [posts](https://www.airpair.com/angularjs/posts/angularjs-performance-large-applications) about [fixing performance issues](http://blog.scalyr.com/2013/10/angularjs-1200ms-to-35ms/) or [ways to avoid pitfalls](http://www.alexkras.com/11-tips-to-improve-angularjs-performance/). Theres no hiding the fact that the not-so-secret secret of Angular is that it is slow. Anyone who has ever done a big Angular application or even writing an [`ng-repeat`](https://docs.angularjs.org/api/ng/directive/ngRepeat) inside an `ng-repeat` can attest to it's slowness.

---

A Google search of the keywords "angular performance" today produces a link to a Github project called [ngReact](https://github.com/ngReact/ngReact). ngReact is a open source project to use [React](https://facebook.github.io/react/) Components in Angular by replacing the view component and keeping the rest of the Angular functionality.

> Facebook's React library is designed to be used as a view component atop other JavaScript frameworks. 
> NgReact is a small library providing helpers and showing examples of achieving interoperability between React and Angular.

This led to my journey to creating a benchmark of sorts to prove that React indeed does add a performance boost to Angular. I started off with the ngReact project to facilitate the integration but ultimately done in a "vanilla" fashion since ngReact has still added some overhead in not only execution time but also in the weight that the project brings in terms of file size. Today the vanilla Angular + React project is hosted in Github in my repository [chrisrng/angular-react-poc](https://github.com/chrisrng/angular-react-poc).

## Benchmarking

To create a viable benchmark, the comparison should be tested on Angular's worst areas since these are the only places where we would inject React to supplement the Angular Framework. The worst area is of course, using an `ng-repeat` inside an `ng-repeat`.

An example of a use case where one would use an  `ng-repeat` within an `ng-repeat` is when creating [Pascal's Triangle](https://en.wikipedia.org/wiki/Pascal%27s_triangle) with elements for each number and an element for the rows. The inner elements are dynamic per row, and the rows are dynamic based on the user's input. This sets a perfect scene for the comparison.

Of course, to do a comparison, we would need to properly measure the loading times of both cases. [HTML5Rocks](http://www.html5rocks.com/en/tutorials/webperformance/usertiming/) provides a great source on using the `window.performance` API. While both Angular and React provides us hooks to fire upon successful rendering.

> "You can’t optimize what you can’t measure" - HTML5Rocks

## Angular-React-PoC

The source can be obtained from the Github repository: [angular-react-poc](https://github.com/chrisrng/angular-react-poc)

Basically both implementations do the same for data setup but only diverges in the displaying of the data. The Angular only way uses an `ng-repeat` inside an `ng-repeat` while the React and Angular way uses mapping on the `render` function to display the dynamic data.

Both have their own ways to diff the data when it is being changed, with Angular using `track by $index` and React using `shouldComponentUpdate`.

Both drastically reduces rendering time when asked to display the same dataset over and over again showing that both ways to diff the data works.

## How to measure performance in loading

Use `window.performance` from the [User Timing API](https://www.w3.org/TR/hr-time/) to have accurate time measurement in milliseconds.

Trigger a start mark on the `scope.load` function call.

Trigger an end mark on the `$timeout` call to mark the end of an Angular render or hook into `componentDidMount` to mark the end of a React render.

### Setup:
- When the HTML is launched, it automatically does a load of 100 rows of pascal's triangle.
- Every time you click on a "Load X" button, it does a performance mark of loading a new pascal's triangle on top of the old one with X rows.
- Thus if repeatedly clicking on the same "Load X" button, the time it takes to render is significantly less (because of the virtual dom or using track by)

## Results

Averaging at around `39.07%` increase in performance over 10, 50, and 100 rows of pascal's triangle.

It did however add a cost of around 39KB of code (react and reactdom) when gzipped and minified, along with the framework initialization costs.

### Angular performance:

![311.315ms for 100 rows](https://raw.github.com/chrisrng/angular-react-poc/master/images/angular-only.png)

### Angular + React performance:

![81.435ms for 100 rows](https://raw.github.com/chrisrng/angular-react-poc/master/images/angular-react.png)
