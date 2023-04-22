<template>
<footer class="bg-light text-center text-lg-start">
  <div class="container p-4 pb-0">
    <form action="">
      <div class="row">
        <div class="col-auto mb-4 mb-md-0">
          <p class="pt-2">
            <strong>Sign up for our newsletter</strong>
          </p>
        </div>

        <div class="col-md-5 col-12 mb-4 mb-md-0">
          <!-- Email input -->
          <div class="form-outline mb-4">
            <input type="email" id="email_id" class="form-control" aria-placeholder="Enter email"/>
            <label class="form-label" for="form5Example25"></label>
          </div>
        </div>

        <div class="col-auto mb-4 mb-md-0">
          <!-- Submit button -->
          <button type="submit" class="btn btn-primary mb-4">
            Subscribe
          </button>
        </div>
      </div>
    </form>
  </div>

  <!-- Copyright -->
  <div class="text-center p-3" style="background-color: rgba(0, 0, 0, 0.2);">
    © 2020 Copyright:
    <a class="text-dark" href="http://localhost:8080/">Group 6</a>
  </div>
  <!-- Copyright -->
</footer>
</template>

<script>
export default {
  name : "Footer"
}
</script>

<style>
footer {
  margin-top : 100px;
  background-color:#232F3E;
  font-size:16px;
}

li {
  padding-left: 0;
  padding-bottom: 10px;
}

a {
  text-decoration: none;
}

.footer-link {
  color: #DDD;
}

footer {
  font-family: 'Roboto', sans-serif;
  text-align: center;
  padding: 35px 0;
}

.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 0 auto;
  max-width: 600px;
}

form {
  width: 100%;
}

.row {
  justify-content: center;
}

.text-center {
  margin-top: 20px;
  padding: 10px 0;
}
</style>
