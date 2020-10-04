
<html lang="en">

<head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css"
        integrity="sha384-JcKb8q3iqJ61gNV9KGb8thSsNjpSL0n8PARn9HuZOnIxN0hoP+VmmDGMN5t9UJ0Z" crossorigin="anonymous">

    <title>TODOs List</title>

    <script>
        let a;
        let date;
        let time;
        const options = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
        setInterval(() => {
        a = new Date();
        date = a.toLocaleDateString(undefined,options);
        time = a.getHours()+':'+a.getMinutes()+':'+a.getSeconds();
        document.getElementById('time').innerHTML = time +"<br>"+date;
    }, 1000);

    </script>
</head>

<body>

    <div class="container my-4">

        <div class="jumbotron">
            <h1 class="display-5">Current t!me is: <span id="time"></span></h1>
            <p class="lead">
                a man who dares to waste one hour of time has not discovered the value of life. ~ Charles Darwin</p>
            <hr class="my-55">
          </div>
      </div>

    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <a class="navbar-brand" href="#">TODOs List</a>
        <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarSupportedContent"
            aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav mr-auto">
                <li class="nav-item">
                    <a class="nav-link"  href="https://github.com/abhishek-netizen">About me</a>
                </li>
            </ul>
        </div>
    </nav>

    <div class="container my-4">
        <h2 class="text-center">TODOs list</h2>

            <div class="form-group">
              <label for="title">Title</label>
              <input type="text" class="form-control" id="title" aria-describedby="emailHelp">
              <small id="emailHelp" class="form-text text-muted">Add an item to the list.</small>
            </div>
            <div class="form-group">
              <!-- <label for="description">Description</label> -->
              <!-- <input type="text" class="form-control" id="description"> -->

              <div class="form-group">
                <label for="description">Description</label>
                <textarea class="form-control" id="description" rows="3"></textarea>
              </div>


            </div>
         
            <button  id="add" class="btn btn-primary">Add to list</button>
            <button  id="clear" class="btn btn-primary" onclick="clearStorage()">Clear list</button>


          <div id="items" class="my-4">
              <h2>Your Items</h2>

              <table class="table">
                <thead>
                  <tr>
                    <th scope="col">SNo</th>
                    <th scope="col">Item Title</th>
                    <th scope="col">Item Description</th>
                    <th scope="col">Actions</th>
                  </tr>
                </thead>
                <tbody id="tableBody">
                  <tr>
                    <th scope="row">1</th>
                    <td>Get some cofee</td>
                    <td>Get some cofee as you are a coder</td>
                    <td><button class="btn btn-sm btn-primary">Delete</button></td>
                  </tr>
            
                </tbody>
              </table>
          </div>
    </div>

    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"
        integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj"
        crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.1/dist/umd/popper.min.js"
        integrity="sha384-9/reFTGAW83EW2RDu2S0VKaIzap3H66lZH81PoYlFhbGU+6BZp6G7niu735Sk7lN"
        crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"
        integrity="sha384-B4gt1jrGC7Jh4AgTPSdUtOBvfO8shuf57BaghqFfPlYxofvL8/KUEfYiJOMMV+rV"
        crossorigin="anonymous"></script>

        <script>
            function getAndUpdate(){
                console.log("updating list..")
                tit = document.getElementById('title').value;
                desc = document.getElementById('description').value;
                if (localStorage.getItem('itemsJson')==null){
                    itemJsonArray = [];
                    itemJsonArray.push([tit,desc]);
                    localStorage.setItem('itemsJson',JSON.stringify(itemJsonArray));
                }
                else{
                    itemJsonArrayStr = localStorage.getItem('itemsJson')
                    itemJsonArray = JSON.parse(itemJsonArrayStr);
                    itemJsonArray.push([tit,desc]);
                    localStorage.setItem('itemsJson',JSON.stringify(itemJsonArray));
                }
                update();

            }
                function update(){
                    if (localStorage.getItem('itemsJson')==null){
                    itemJsonArray = [];
                    localStorage.setItem('itemsJson',JSON.stringify(itemJsonArray));
                }
                else{
                    itemJsonArrayStr = localStorage.getItem('itemsJson')
                    itemJsonArray = JSON.parse(itemJsonArrayStr);
                }

                    //populate the table
                let tableBody = document.getElementById("tableBody");
                let str="";
                itemJsonArray.forEach((element,index) => {
                    str += `
                    <tr>
                    <th scope="row">${index + 1}</th>
                    <td>${element[0]}</td>
                    <td>${element[1]}</td>
                    <td><button class="btn btn-sm btn-primary" onclick="deleted(${index})">Delete</button></td>
                   </tr>`;
    
                });
                tableBody.innerHTML = str;


            }

            add = document.getElementById("add");
            add.addEventListener("click",getAndUpdate);
            update();
            function deleted(itemIndex){
                console.log("delete",itemIndex);
                itemJsonArrayStr = localStorage.getItem('itemsJson')
                itemJsonArray = JSON.parse(itemJsonArrayStr);
              //delete item index from the array
              itemJsonArray.splice(itemIndex,1)
                localStorage.setItem('itemsJson',JSON.stringify(itemJsonArray));
                update();
            }
            function clearStorage(){
                if (confirm("Do you really want to clear all the Notes!?")){
                console.log("clearing the storage")
                localStorage.clear();
                update();
            }
            }

        </script>

</body>

</html>
