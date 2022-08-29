# TIL testing GPOs without affecting everybody

Spent a lot of time off and on trying to find a reasonable way to test GPOs without affecting everybody. I think most of my confusion stems from `Computer Configuration` vs. `User Configuration`.

Two ways to do this.

1. Create a `test-ou` and put the computer and/or user in this ou.
2. Security Filtering. Remove `Apply group policy` from `Authenticated Users` and add a User or Group with `Apply group policy`.

# I've learned

 * Run `gpupdate /force` to update local GPOs. By default windows applies them every 15 minutes.
 * Run `gpresult /h gpreport.html` or `gpresult /r` to find out what got applied. If you're interested in `User Configuration` be sure to run it as the user. If you're interested in `Computer Configuration` run it as admin.
 * If only the computer gets moved into the `test-ou`, configuration from `User Configuration` does not get applied. And vice versa.
