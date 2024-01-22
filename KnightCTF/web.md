# Solution
## Task 1: Kitty
Question: Tetanus is a serious, potentially life-threatening infection that can be transmitted by an animal bite.

We are given a login page, so I analyzed the page source and stumbled across the Javascript file. It says that the credentials are `username` and `password` (Not very secure lol).

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/af4ad084-579e-48c8-9f55-33f4d4cea505)

```
document.getElementById('login-form').addEventListener('submit', function(event) {
    event.preventDefault();

    const username = document.getElementById('username').value;
    const password = document.getElementById('password').value;

    const data = {
        "username": username,
        "password": password
    };

    fetch('/login', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json'
        },
        body: JSON.stringify(data)
    })
    .then(response => response.json())
    .then(data => {
        // You can handle the response here as needed
        if (data.message === "Login successful!") {
            window.location.href = '/dashboard'; // Redirect to the dashboard
        } else {
            // Display an error message for invalid login
            const errorMessage = document.createElement('p');
            errorMessage.textContent = "Invalid username or password";
            document.getElementById('login-form').appendChild(errorMessage);

            // Remove the error message after 4 seconds
            setTimeout(() => {
                errorMessage.remove();
            }, 4000);
        }
    })
    .catch(error => {
        console.error('Error:', error);
    });
});
```

After logging it, we are redirected to another page, the dashboard. Same thing I analyzed the page source and found a script field.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/ee5c4363-b538-4203-a853-5c91cdcbb6b8)

```
<script>
    function addPost(event) {
        event.preventDefault();
        const post_in = document.getElementById('post_input').value;
        
        if (post_in.startsWith('cat flag.txt')) {
            fetch('/execute', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/x-www-form-urlencoded'
                },
                body: `post_input=${encodeURIComponent(post_in)}`
            })
            .then(response => response.text())
            .then(result => {
                const contentSection = document.querySelector('.content');
                const newPost = document.createElement('div');
                newPost.classList.add('post');
                newPost.innerHTML = `<h3>Flag Post</h3><p>${result}</p>`;
                contentSection.appendChild(newPost);
            });
        } else {
            const contentSection = document.querySelector('.content');
            const newPost = document.createElement('div');
            newPost.classList.add('post');
            newPost.innerHTML = `<h3>User Post</h3><p>${post_in}</p>`;
            contentSection.appendChild(newPost);
        }
    }
</script>
```

The script mentioned by using sending a command `cat flag.txt` in the input field, we can get the flag.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/782661e9-07fb-47b0-aebc-15cbac9d82c8)

## Task 2: Levi Ackerman
Question: Levi Ackerman is a robot!

We are given an image of Levi in the website, nothing else. So by reading the question, I did the common tactic in Web CTFs which is navigating to robots.txt.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/28351273-09c4-48a0-8fc3-505d41b94200)

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/b66db858-ec5d-4857-970b-d24022204a9d)

We can see that it allows this URL, so we access it and get the flag.

![image](https://github.com/warlocksmurf/ctftime-writeups/assets/121353711/0dcdc787-202c-4a7e-b4bf-251d59b618ef)
