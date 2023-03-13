Kill Process -
N: total id, n: the id to delete
construct tree O(N) first, then O(n) to delete  vs. bfs with children map, O(N) to find children each level, total O(Nn)




645. Set Mismatch

Given 1..n
N numbers, theres one duplicate, thus one missing. find the two.

Interesting idea:
m, d

m - d we can easily find.
m^2 - d^2 can be found in O(N)

then we basically have m + d,
then we can have each.


Following what @joseville said,
Given: d represents the the duplicate element and m represents the missing element
diff = m-d
squareDiff = m^2 - d^2 = (m+d) * (m-d)
We have:
sum = squareDiff/diff = m+d
(sum - diff) / 2 = (m+d - (m-d)) / 2 = d
(sum + diff) / 2 = (m+d+ (m-d)) / 2 = m

My idea:
swap sort forward, if see duplicate, mark as -1 and note its location


683. K Empty Slots

The idea is to use an array days[] to record each position's flower's blooming day. That means  days[i] is the blooming day of the flower in position i+1. We just need to find a subarray days[left, left+1,..., left+k-1, right] which satisfies: for any i = left+1,..., left+k-1, we can have days[left] < days[i] && days[right] < days[i]. Then, the result is max(days[left], days[right]).
