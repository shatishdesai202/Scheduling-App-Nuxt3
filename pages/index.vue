<template>
    <div class="login-container">
        <h1>Login</h1>
        <form @submit.prevent="login">
            <div class="form-group">
                <label for="username">Username</label>
                <input type="text" id="username" name="username" required>
            </div>
            <div class="form-group">
                <label for="password">Password</label>
                <input type="password" id="password" name="password" required>
            </div>
            <button type="submit">Login</button>
        </form>
    </div>
</template>
  
<script>
export default {
    methods: {
        async login() {
            // Get the username and password from the form
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;

            // Authenticate the user
            const isValid = await this.authenticate(username, password);

            // If authentication is successful, redirect to the dashboard page
            if (isValid == 0) {
                alert('username or password are wrong!')
             
            }
        },

        navigate() {
            return navigateTo({
                path: '/dashboard',
            })
        },

        async authenticate(username, password) {
            if (username === 'admin' && password === 'password') {
                this.navigate()
                localStorage.setItem("loginType", "1")
                return 1;
            } else if (username === 'employee' && password === 'password') {
                this.navigate()
                localStorage.setItem("loginType", "2")
                return 2;
            } else if (username === 'user1' && password === 'password') {
                this.navigate()
                localStorage.setItem("loginType", "3")
                return 3;
            } else if (username === 'user2' && password === 'password') {
                this.navigate()
                localStorage.setItem("loginType", "3")
                return 3;
            } else {
                return 0;
            }
        }
    }
}
</script>
  
  
<style scoped>
.login-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
}

form {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    max-width: 400px;
    padding: 2rem;
    background-color: #f0f0f0;
    border-radius: 10px;
}

.form-group {
    display: flex;
    flex-direction: column;
    margin-bottom: 1rem;
}

label {
    font-weight: bold;
    margin-bottom: 0.5rem;
}

input[type="text"],
input[type="password"] {
    padding: 0.5rem;
    border-radius: 5px;
    border: none;
    margin-bottom: 1rem;
}

button[type="submit"] {
    padding: 0.5rem;
    border-radius: 5px;
    border: none;
    background-color: #0070f3;
    color: white;
    cursor: pointer;
    transition: all 0.2s ease-in-out;
}

button[type="submit"]:hover {
    background-color: #0062cc;
}
</style>
  