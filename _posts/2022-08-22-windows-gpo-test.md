# TIL testing GPOs without affecting everybody

Spent a lot of time off and on trying to find a reasonable way to test GPOs without affecting everybody. I think most of my confusion stems from `Computer Configuration` vs. `User Configuration`.

I've created a `test-ou`.

# I've learned

 * Run `gpupdate /force` to update local GPOs. By default windows applies them every 15 minutes.
 * Run `gpresult /h gpreport.html` as admin to find out what got applied.
 * If only the computer gets moved into the `test-ou`, configuration from `User Configuration` does not get applied. And vice versa.
