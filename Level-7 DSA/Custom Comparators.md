Ascending( Default )  => Arrays.sort(a, b);
Descending => Arrays.sort(arr, Collections.reverseOrder())

Even Numbers => (a, b) -> (a % 2) - (b % 2)
Absolute Value Sort => (a,b) -> Math.abs(a) - Math.abs(b);
Custom Descending => (a, b) -> b - a
sort by first column => (a,b) -> a[0] - b[0]
sort by second column => (a,b) -> a[1] - b[1]
sort by sum => (a,b) -> (a[0] + b[0]) - (b[0] + b[1])
sort by difference => (a,b) -> (a[0] - b[0]) - (b[0] - b[1])

Multi-condition sort
Arrays.sort(arr, (a,b) -> {
	if(a[0] == b[0]) return a[1] - b[1];
	return a[0] - b[0];
	});
