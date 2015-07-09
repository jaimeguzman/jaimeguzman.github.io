##Subl command not working - command not found##

I testing this on OSX, is in my todoist create the same symbolink on my ubuntu machine
First create the symlink in 

> /usr/local/bin instead of ~/bin 

and make sure that is in you PATH

> /usr/local/binin  


      $ ln -s /Applications/Sublime\ Text.app/Contents/SharedSupport/bin/subl /usr/local/bin/.
      $ echo $PATH
      If you don't find /usr/local/bin/, then add the following lines to your .bashrc or .zshrc
      
      PATH=$PATH:/usr/local/bin/; export PATH



http://stackoverflow.com/questions/25152711/subl-command-not-working-command-not-found
