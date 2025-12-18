…or create a new repository on the command line
echo "# linux-monitoring-stack" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:WEPGT/linux-monitoring-stack.git
git push -u origin main


…or push an existing repository from the command line
git remote add origin git@github.com:WEPGT/linux-monitoring-stack.git
git branch -M main
git push -u origin main




git add -A
git commit -m "Remove large files from repository"
git push

git ls-files
This shows everything Git is tracking.