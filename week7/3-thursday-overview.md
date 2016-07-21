Overview for Thursday, July 21st
================================

Sorting/Filtering/Searching
---------------------------
We can do sorting and filtering/searching either client-side or server-side.

Let's go over some of the highlights of each method:

##### Client-side
- Extremely easy to implement with Angular filters
- Doesn't require any server-side changes or code
- Works great for small datasets (<500 rows)

##### Server-side
- Sorting/filtering is done server-side
- Dataset can be millions of rows
- Pagination is easy to setup


### Client-side Sorting/Filtering
Angular *filters* (we've already seen the `date` filter) allow us to easily add sorting and filtering to any of our `ng-repeat` directives.

Let's create a fictional dataset and add it to scope.  This is just an array of items, like we would get from our server-side mongo queries.

```js
$scope.items = [
	{id:1,	name:'First Item',		color:'red',	size:'large'},
	{id:2,	name:'Second Item',		color:'green',	size:'small'},
	{id:3,	name:'Third Item',		color:'blue',	size:'medium'},
	{id:4,	name:'Fourth Item',		color:'while',	size:'tiny'},
	{id:5,	name:'Fifth Item',		color:'black',	size:'huge'},
	{id:6,	name:'Sixth Item',		color:'gray',	size:'maximum'},
	{id:7,	name:'Seventh Item',	color:'orange',	size:'minimum'},
	{id:8,	name:'Eighth Item',		color:'purple',	size:'gigantic'},
	{id:9,	name:'Ninth Item',		color:'brown',	size:'pico'},
	{id:10,	name:'Tenth Item',		color:'pink',	size:'nano'}
];
```

Our `ng-repeat` looks like this without sorting or filtering:

```html
<table>
	<tr ng-repeat="item in items">
		<td>{{item.id}}</td>
		<td>{{item.name}}</td>
		<td>{{item.color}}</td>
		<td>{{item.size}}</td>
	</tr>
</table>
```

#### `orderBy` Filter

The `orderBy` filter allows us to sort any array of items by a property of those items.

The format is `orderBy:property:reverse`.  `reverse` is an optional boolean type, setting it `true` reverses the sort direction.

Let's order our example dataset, `$scope.items`, by the `color` property:

```html
<tr ng-repeat="item in items | orderBy:'color'">
```

See [examples/sorting-filtering/sorting1.html](https://github.com/sergei202/okcoders-class/tree/master/week7/examples/sorting-filtering/sorting1.html) for a example of sorting by a given property.  Play around with different properties.

Notice that the first argument to the `orderBy` filter (colons separate arguments for filters) is in single quotes.  This tells us that it takes an `expression`.  This means we can give it a scope variable instead of hardcoding a value!

Let's add a `<input>` tag that is bound to the scope variable `mySortProp`:

```html
<input ng-model="mySortProp">

<table>
	<tr ng-repeat="item in items | orderBy:mySortProp">
		<td>{{item.id}}</td>
		<td>{{item.name}}</td>
		<td>{{item.color}}</td>
		<td>{{item.size}}</td>
	</tr>
</table>
```

We can now type whatever property we want our table sorted by!  Check out [examples/sorting-filtering/sorting2.html](https://github.com/sergei202/okcoders-class/tree/master/week7/examples/sorting-filtering/sorting2.html) for an example of this.  Try typing in `id`, `name`, `size`, or `-color`.  Adding a hyphen in front reverses the sort order.

Let's go a step further and give the user a dropdown (a `<select>` box) to pick the sort field and another for the direction:

```html
<select ng-model="sortProp"     ng-options="o.prop as o.label for o in sortProps"></select>
<select ng-model="sortReverse"  ng-options="o.rev  as o.label for o in sortDirs"></select>
...
<tr ng-repeat="item in items | orderBy:sortProp:sortReverse">
```

We'll need to define `sortProps` and `sortDirs` in our controller:

```js
$scope.sortProps = [				// Sort properties that we allow the user to sort by
	{prop:'id',		label:'Sort by ID'},
	{prop:'name',	label:'Sort by Name'},
	{prop:'color',	label:'Sort by Color'},
	{prop:'size',	label:'Sort by Size'}
];
$scope.sortDirs = [					// Sort directions
	{rev:false,	label:'Forwards'},
	{rev:true,	label:'Backwards'}
];
```

See [examples/sorting-filtering/sorting3.html](https://github.com/sergei202/okcoders-class/tree/master/week7/examples/sorting-filtering/sorting3.html) for an example of the above.