# How to Build

cd lab-workbook
mkdocs build
docker build -t ncp .
docker run -p 8080:80 ncp


cd lab-workbook/site
python3 -m http.server 8000
Then open http://localhost:8000

cd ~/repos/hol/ncp/lab-workbook && vercel

