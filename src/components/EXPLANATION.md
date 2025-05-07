## First Issue - Variable not reactive
The first issue identified is the lack of reactivity in the input or any event being triggered.
To address this, some strategies could be used, the most common being either adding a button to trigger a state change,
or making the variable reactive. I chose the reactive approach since the structure was nearly ready, only missing the 
ref on the limit declaration.

**Note: Since limit becomes a reference, we must access its value using .value, like limit.value.**
## Second Issue - Input validation
After making it reactive, I noticed another missing part of the logic: the input should be limited between 1 and 100.
This can be enforced directly in the HTML input, but to prevent value injection via the HTML,
I also added this constraint inside the `n()` function. This ensures that even if the value is manipulated directly,
it remains within the expected range. (A `watch()` could also be used for this, but I found this approach safer)
I also used the change event as an extra layer of validation.

## Third Issue - 0 being displayed and 100 isn't
The third problem is that the rule clearly states that numbers should be displayed from 1 to 100 based on the input,
but we see a 0 being displayed. This occurs because the loop in the `n()` function starts at 0 instead of 1.
Additionally, the final number is capped at 99 because the condition checks if the index is less than `nums.length`,
excluding the upper limit.

## Fourth Issue - Summing with String type
I found that the constant num in the hov(number) function is a string.
Although it works because JavaScript does implicit type casting, this is bad practice.
It could lead to NaN in non-numeric scenarios, for instance, if the user changes the input type from number 
to text via the browser. To avoid that, I applied proper parsing.

## Fifth Issue - n() being utilized multiple times
The `n()` function is used multiple times in the template, which is not necessary. I made it a computed property.

## Sixth Issue - Bad way of populating the array
The `...` (*spread operator*) is being used to rebuild the array repeatedly, but in this case, it's unnecessary.
To optimize the operation, I'll use `.push()` instead.