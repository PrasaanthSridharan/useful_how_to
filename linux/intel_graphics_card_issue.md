Screen tearing issues reference links: 
- https://askubuntu.com/questions/945895/solution-to-intel-graphics-screen-tearing-flickering-causes-excessive-fan-use
- https://askubuntu.com/questions/1033665/strange-graphics-issue-after-upgrading-to-18-04-that-affects-only-one-user-accou
- https://askubuntu.com/questions/1065852/how-to-upgrade-intel-graphics-driver (This appears to be the one that fixed it)


The solution was to add the following repo: 
```
sudo add-apt-repository ppa:oibaf/graphics-drivers
```

After performing the adding, the following two commands were performed that fixed the issue: 
```
sudo apt-get update
sudo apt-get upgrade
```

After performing a restart, there appear to be no glitches so far. 