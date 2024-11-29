# **Quadratic and System Solver Application**

This is a terminal-based Python application that solves quadratic equations and systems of equations (with 2 or 3 unknowns) using the **Wolfram Alpha Short Answers API**. The application can be run locally or deployed on web servers with a load balancer to handle traffic efficiently.

---

## **Features**

1. **Solve Quadratic Equations**: Solves equations of the form \( ax^2 + bx + c = 0 \).
2. **Solve Systems of 2 Equations**: Handles linear systems with two unknowns.
3. **Solve Systems of 3 Equations**: Solves systems with three unknowns.
4. **Instructions**: Provides detailed instructions on usage and expected input.
5. **Exit Option**: Allows users to exit the application gracefully.

---

## **APIs Used**

- **Wolfram Alpha Short Answers API**  
  - The app utilizes this API to fetch solutions for mathematical queries.
  - [Official Documentation](https://products.wolframalpha.com/short-answers-api/documentation/)

---

## **Technologies**

- **Python 3**
- **Gunicorn** (for web server functionality)
- **Nginx** (as a reverse proxy)
- **HAProxy** (as a load balancer)

---

## **Prerequisites**

- Python 3.6+
- Access to two web servers and one load balancer
- A Wolfram Alpha API key (added to `.env` file)

---

## **Setup Instructions**

### **Running the Application Locally**

1. Clone the repository:
   ```bash
   git clone https://github.com/maajabu-alleluia-tito/your-repo.git
   cd your-repo
   ```

2. Create a Python virtual environment and activate it:
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. Install the dependencies:
   ```bash
   pip install -r requirements.txt
   ```

4. Create a `.env` file in the project root:
   ```
   WOLFRAM_APP_ID=your_wolfram_alpha_app_id
   ```

5. Run the application:
   ```bash
   python app.py
   ```

6. Follow the menu prompts to solve equations or view instructions.

---

### **Deploying to Web Servers**

#### **Step 1: Prepare the Web Servers**

1. SSH into each server:
   ```bash
   ssh ubuntu@<web-server-ip>
   ```

2. Update packages and install dependencies:
   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo apt install python3 python3-pip python3-venv nginx -y
   ```

3. Clone the repository and set up the environment:
   ```bash
   git clone https://github.com/your-username/your-repo.git
   cd your-repo
   python3 -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   ```

4. Set up Gunicorn:
   ```bash
   gunicorn --bind 0.0.0.0:8000 app:main
   ```

5. Configure Nginx as a reverse proxy. Add this to `/etc/nginx/sites-available/app`:
   ```nginx
   server {
       listen 80;
       server_name <server-ip>;

       location / {
           proxy_pass http://127.0.0.1:8000;
           proxy_set_header Host $host;
           proxy_set_header X-Real-IP $remote_addr;
       }
   }
   ```
   Enable the configuration and restart Nginx:
   ```bash
   sudo ln -s /etc/nginx/sites-available/app /etc/nginx/sites-enabled/
   sudo nginx -t
   sudo systemctl restart nginx
   ```

#### **Step 2: Configure the Load Balancer**

1. SSH into the load balancer:
   ```bash
   ssh ubuntu@<load-balancer-ip>
   ```

2. Install and configure HAProxy:
   ```bash
   sudo apt install haproxy -y
   sudo nano /etc/haproxy/haproxy.cfg
   ```

3. Add the following configuration:
   ```haproxy
   frontend http_front
       bind *:80
       default_backend http_back

   backend http_back
       balance roundrobin
       server web01 <web-01-ip>:80 check
       server web02 <web-02-ip>:80 check
   ```
4. Restart HAProxy:
   ```bash
   sudo systemctl restart haproxy
   ```

---

## **Challenges Encountered**

- **Issue with API Responses**: Initial attempts to solve systems of equations using raw input failed. This was resolved by requiring users to input coefficients rather than complete equations, ensuring query formatting aligned with the API's expectations.
- **Load Balancer Configuration**: Setting up HAProxy to evenly distribute requests required careful testing and fine-tuning.
- **Input Validation**: Implemented robust validation using regex to handle incorrect inputs gracefully.

---

## **Credits**

- **Wolfram Alpha** for their powerful computational engine and API.
- **Python Community** for extensive documentation and libraries.
- **Stack Overflow** for troubleshooting deployment and configuration issues.

---



