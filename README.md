# Food-App
I.
package com.foodapp;

import java.io.*;
import java.util.ArrayList;
import java.util.List;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

@WebServlet("/add-to-cart")
public class FoodAppServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        HttpSession session = request.getSession();
        @SuppressWarnings("unchecked")
        List<String> cart = (List<String>) session.getAttribute("cart");
        if (cart == null) {
            cart = new ArrayList<>();
        }

        String item = request.getParameter("item");
        String price = request.getParameter("price");
        cart.add(item + " - ₹" + price);
        session.setAttribute("cart", cart);

        response.getWriter().write("Item added to cart");
    }
}



II.
package com.foodapp;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import jakarta.servlet.http.HttpSession;

@WebServlet("/get-cart")
public class GetCartServlet extends HttpServlet {
    private static final long serialVersionUID = 1L;

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        HttpSession session = request.getSession();
        @SuppressWarnings("unchecked")
        List<String> cart = (List<String>) session.getAttribute("cart");

        if (cart == null) {
            cart = new ArrayList<>();
        }

        StringBuilder json = new StringBuilder("[");
        for (int i = 0; i < cart.size(); i++) {
            json.append("\"").append(cart.get(i)).append("\"");
            if (i < cart.size() - 1) {
                json.append(",");
            }
        }
        json.append("]");

        response.setContentType("application/json");
        response.getWriter().write(json.toString());
    }
}


III.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Your Cart</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <div class="container">
            <h1>Your Cart</h1>
        </div>
        <nav>
            <ul>
                <li><a href="project1.html">HOME</a></li>
            </ul>
        </nav>
    </header>
    <section id="cart-items">
        <h2>Items in Your Cart</h2>
        <ul id="cart-list"></ul>
        <p><strong>Total Amount: ₹<span id="total-amount">0</span></strong></p>
        <button id="clear-cart">Clear Cart</button>
        <button id="checkout">Proceed to Checkout</button>
        <button id="add-item">Add Another Item</button>
    </section>
    <footer>
        <p>&copy; 2024 Foodie Corner</p>
    </footer>
    <script>
        const cart = JSON.parse(localStorage.getItem("cart")) || [];
        const cartList = document.getElementById("cart-list");
        const totalAmountElement = document.getElementById("total-amount");

        let totalAmount = 0;

        if (cart.length > 0) {
            cart.forEach(item => {
                const li = document.createElement("li");
                li.textContent = `${item.item} - ₹${item.price}`;
                cartList.appendChild(li);
                totalAmount += parseInt(item.price);
            });
            totalAmountElement.textContent = totalAmount;
        } else {
            cartList.innerHTML = "<li>Your cart is empty.</li>";
        }

        document.getElementById("clear-cart").addEventListener("click", () => {
            localStorage.removeItem("cart");
            window.location.reload();
        });

        document.getElementById("checkout").addEventListener("click", () => {
            if (cart.length > 0) {
                alert(`Payment Successful! Total Amount: ₹${totalAmount}`);
                localStorage.removeItem("cart");
                window.location.href = "project1.html";
            } else {
                alert("Your cart is empty! Add items before checking out.");
            }
        });

        document.getElementById("add-item").addEventListener("click", () => {
            window.location.href = "project1.html";
        });
    </script>
</body>
</html>


IV.
<%@ page import="java.util.*" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>FoodApp - Cart</title>
</head>
<body>
    <h1>Your Cart</h1>
    <ul>
        <% 
            // Retrieve cart items passed from the servlet
            Object cartObj = request.getAttribute("cart");
            if (cartObj instanceof List) {
                List<String> cart = (List<String>) cartObj;
                if (!cart.isEmpty()) {
                    for (String item : cart) {
                        out.println("<li>" + item + "</li>");
                    }
                } else {
                    out.println("<li>Your cart is empty.</li>");
                }
            } else {
                out.println("<li>Your cart is empty.</li>");
            }
        %>
    </ul>
    <a href="project1.html">Back to Menu</a>
</body>
</html>

V.
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Food App</title>
    <link rel="stylesheet" href="styless.css">
    <link href='https://unpkg.com/boxicons@2.1.4/css/boxicons.min.css' rel='stylesheet'>
</head>
<body>
   
        <header>
            
                <div class="container">
                   
                        <h1>The Saturday's</h1>
                    </div>
  <style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
body{
    display:flex;
    justify-content: center;
    align-items: center;
  min-height: 100vh;
    background:url('snoo.jpeg');
    background-position: center;
    background-size: cover;
 }

.wrapper{
    width: 420px;
   background-color:bisque;
   border: 2px solid rgba(39, 37, 37, 0.2);
    color: #1d1c1c;
    border-radius: 10px;
    padding: 30px 40px;

}
.wrapper h1{
    font-size: 36px;
    text-align: center;
}
 .wrapper .input-box{
    position: relative;
    width: 100%;
    height: 50px;
  
    margin: 40px 0;
}
 .input-box input{
    width: 100%;
    height: 100%;
    
    background: transparent;
    border: none;
    outline: none;
    border: 2px solid rgba(55, 52, 52, 0.2);
    border-radius: 40px;
    font-size: 16px;
    color:black;
    padding: 20px 10px 10px ;
}
 
 .input-box input::placeholder{
    color:#1a1818;
}
.input-box i{
    position: absolute;
    right: 20px;
    top:50%;
    transform: translate(-50%);
    font-size: 20pxs;

}
 .wrapper .btn{
    width: 80%;
    height: 55px;
    background: #fff;
    border: none;
    outline: none;
    border-radius: 40px;
    margin-left: 50px;
    margin-top: 20px;
    cursor: pointer;
    font-size: 16px;
    color: black;
    font-weight: 600;
}


</style>
           
          
            <div class="wrapper">
                <form action="">
                  <h1>Login</h1>
                  <div class="input-box">
                      <input type="text" placeholder="Username">
                      <i class='bx bxs-user'></i>
                  </div>
                  <div class="input-box">
                      <input type="password" placeholder="password">
                      <i class='bx bxs-lock-alt'></i>
                  </div>
                  <button type="submit" class="btn">Login</button>
                  
                </form>
                </div>
            </div>
            </header>
            </body>
            </html>

VI.

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Food Menu</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <div class="container">
            <h1>The Saturday's</h1>
        </div>
        <nav>
            <ul>
                <li><a href="login.html">Login</a></li>
            </ul>
        </nav>
    </header>
    <section id="menu">
        <h2>Menu</h2>
        <!-- Menu items -->
        <div class="menu-item">
            <img src="backkk.jpg" alt="Nut Brownie">
            <h3>Nut Brownie</h3>
            <p>Price: ₹50</p>
            <button class="add-to-cart" data-item="Nut Brownie" data-price="50">Add to Cart</button>
        </div>
                <div class="menu-item">
            <img src="chochomoun.jpeg" alt="Choco Filled Cup">
            <h3>Choco Filled Cup</h3>
            <p>Price: ₹80</p>
            <button class="add-to-cart" data-item="Choco Filled Cup" data-price="80">Add to Cart</button>
        </div>
          <div class="menu-item">
            <img src="cshake.jpg" alt="Shake">
            <h3>Choco Shake</h3>
            <p>Price: ₹40</p>
            <button class="add-to-cart" data-item="Choco Shake" data-price="40">Add to Cart</button>
        </div>
           <div class="menu-item">
            <img src="matcha.jpeg" alt="Matcha">
            <h3>Matcha Pastry</h3>
            <p>Price: ₹100</p>
            <button class="add-to-cart" data-item="Matcha Pastry" data-price="100">Add to Cart</button>
        </div>
                        <div class="menu-item">
            <img src="pan.jpeg" alt="Pancake">
            <h3>Pancake</h3>
            <p>Price: ₹80</p>
            <button class="add-to-cart" data-item="Pancake" data-price="80">Add to Cart</button>
        </div>
                        <div class="menu-item">
            <img src="pasta0.jpg" alt="pasta">
            <h3>Pasta</h3>
            <p>Price: ₹100</p>
            <button class="add-to-cart" data-item="Pasta" data-price="100">Add to Cart</button>
        </div>
        
       
    </section>
    <footer>
        <p>&copy; 2024 Foodie Corner</p>
    </footer>
    <script src="script.js"></script>
</body>
</html>


document.querySelectorAll(".add-to-cart").forEach(button => {
    button.addEventListener("click", () => {
        const item = button.getAttribute("data-item");
        const price = button.getAttribute("data-price");

        let cart = JSON.parse(localStorage.getItem("cart")) || [];
        cart.push({ item, price });
        localStorage.setItem("cart", JSON.stringify(cart));

        window.location.href = "cart.html";
    });
});

VII.

body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f8f8f8;
}

h1, h2, h3 {
    color: #333;
}

header {
    background-color: #333;
    color: white;
    padding: 20px;
}

header .container {
    max-width: 1200px;
    margin: 0 auto;
    text-align: center;
}

header nav ul {
    list-style-type: none;
    padding: 0;
}

header nav ul li {
    display: inline;
    margin-right: 20px;
}

header nav ul li a {
    color: white;
    text-decoration: none;
    font-size: 16px;
}

#menu {
    max-width: 1200px;
    margin: 20px auto;
    text-align: center;
}

.menu-item {
    display: inline-block;
    width: 200px;
    padding: 20px;
    margin: 20px;
    background-color: white;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    border-radius: 10px;
}

.menu-item img {
    width: 100%;
    border-radius: 10px;
}

button {
    background-color: #ff6347;
    color: white;
    border: none;
    padding: 10px;
    margin-top: 10px;
    cursor: pointer;
    border-radius: 5px;
    font-size: 14px;
}

button:hover {
    background-color: #ff4500;
}
#cart-items {
    max-width: 1000px;
    margin: 20px auto;
    padding: 20px;
    background-color: white;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    border-radius: 10px;
}

.cart-item {
    border-bottom: 1px solid #ddd;
    padding: 10px 0;
}

.cart-item h3 {
    margin: 0;
}

#clear-cart, #checkout {
    display: inline-block;
    background-color: #ff6347;
    color: white;
    border: none;
    padding: 10px 20px;
    margin-top: 20px;
    cursor: pointer;
    border-radius: 5px;
    font-size: 16px;
}

#clear-cart:hover, #checkout:hover {
    background-color: #ff4500;
}

/* Footer styles */
footer {
    text-align: center;
    background-color: #333;
    color: white;
    padding: 10px;
    position: fixed;
    bottom: 0;
    width: 100%;
}

