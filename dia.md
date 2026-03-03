To install Dia in the MacOS (Venture)), the steps are as follows:

1. Install XQuartz
2. Install Dia (since they website because the brew and port package there it's not anymore here, download the last .dmg)
3. Don't open Dia and close XQuartz if it's opened after of the installation
   3.1. In the case of that you opened Dia before the next steps you need to remove the application and reinstall it. And again do not open it and continue since step 3
4. Edit the file /Applications/Dia.app/Contents/Resources/bin/dia (I suggest to use a terminal and vim and the file to edit is dia not the binary dia-bin)
   4.1. In the case of you don't found that file dia go to step 3.1 and continue since that
5. In the /Applications/Dia.app/Contents/Resources/bin/dia go to the last line and edit that you will see:
   > ... [the previous rest of the file]
   > 
   > exec "dia-bin" --integrated
   > 
   And you need to add this: --display=:0 "$@" the final result it's:
   > exec "dia-bin" --integrated --display=:0 "$@"
   > 
   > Just add --display=:0 "$@" after of the --integrated doesn't matter whatever else. And you got it!
   > 
