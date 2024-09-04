Although i could have used the sort to output files i just use it as a pipe because i like cat.
the random characters are the unique characters hence i used uniq  to only capture them -c to count number  of occurrences.
And lastly grep 1 since i want the one which only appears once.
cat data.txt |sort | uniq -c | grep 1


Password: ==4CKMh1JI91bUIZZPXDqGanal4xvAg0JM==