# CAR-PRICE-PREDICTOR

//Index.html

<html lang="en">
<head xmlns="http://www.w3.org/1999/xhtml">
    <meta charset="UTF-8">
    <title>Car Price Predictor</title>
    <link rel="stylesheet" href="static/css/style.css">
    <link rel="stylesheet" type="text/css"
          href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.11.2/css/all.css">
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js"
            integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo"
            crossorigin="anonymous"></script>
 <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/css/bootstrap.min.css"integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/@tensorflow/tfjs@2.0.0/dist/tf.min.js"></script>
</head>
<body class="bg-dark">
<div class="container">
    <div class="row">
        <div class="card mt-50" style="width: 100%; height: 100%">
            <div class="card-header" style="text-align: center">
                <h1>Welcome to Car Price Predictor</h1>

</div>
            <div class="card-body">
                <div class="col-12" style="text-align: center">
                    <h5>This app predicts the price of a car you want to sell. Try filling the details below: </h5>
                </div>
                <br>
                <form method="post" accept-charset="utf-8" name="Modelform">
                    <div class="col-md-10 form-group" style="text-align: center">
                        <label><b>Select the company:</b> </label><br>
                        <select class="selectpicker form-control" id="company" name="company" required="1"
                                onchange="load_car_models(this.id,'car_models')">
                            {% for company in companies %}
                            <option value="{{ company }}">{{ company }}</option>
                            {% endfor %}
                        </select>
                    </div>
                    <div class="col-md-10 form-group" style="text-align: center">
                        <label><b>Select the model:</b> </label><br>
                        <select class="selectpicker form-control" id="car_models" name="car_models" required="1">
                        </select>
                    </div>

<div class="col-md-10 form-group" style="text-align: center">
                        <label><b>Select Year of Purchase:</b> </label><br>
                        <select class="selectpicker form-control" id="year" name="year" required="1">
                            {% for year in years %}
                            <option value="{{ year }}">{{ year }}</option>
                            {% endfor %}
                        </select>
                    </div>
                    <div class="col-md-10 form-group" style="text-align: center">
                        <label><b>Select the Fuel Type:</b> </label><br>
                        <select class="selectpicker form-control" id="fuel_type" name="fuel_type" required="1">
                            {% for fuel in fuel_types %}
                            <option value="{{ fuel }}">{{ fuel }}</option>
                            {% endfor %}
                        </select>
                    </div>
                    <div class="col-md-10 form-group" style="text-align: center">
                        <label><b>Enter the Number of Kilometres that the car has travelled:</b> </label><br>
                        <input type="text" class="form-control" id="kilo_driven" name="kilo_driven"
                               placeholder="Enter the kilometres driven ">
                    </div>
                    <div class="col-md-10 form-group" style="text-align: center">
                        <button  class="btn btn-primary form-control" onclick="send_data()">Predict Price</button>
</div>
                </form>
                <br>
                <div class="row">
                    <div class="col-12" style="text-align: center">
                        <h4><span id="prediction"></span></h4>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
<script>
function load_car_models(company_id,car_model_id)
    {
        var company=document.getElementById(company_id);
        var car_model= document.getElementById(car_model_id);
        console.log(company.value);
        car_model.value="";
        car_model.innerHTML="";
        {% for company in companies %}
            if( company.value == "{{ company }}"{
                {% for model in car_models %}
        {% if company in model %}
var newOption= document.createElement("option");
 newOption.value="{{ model }}";
  newOption.innerHTML="{{ model }}";
  car_model.options.add(newOption);
 {% endif %}
 {% endfor %}
        {% endfor %}
    }
function form_handler(event) {
        event.preventDefault(); // Don't submit the form normally
    }
    function send_data()
    {
        document.querySelector('form').addEventListener("submit",form_handler);

        var fd=new FormData(document.querySelector('form'));

        var xhr= new XMLHttpRequest({mozSystem: true});

        xhr.open('POST','/predict',true);
        document.getElementById('prediction').innerHTML="Wait! Predicting Price.....";
        xhr.onreadystatechange = function(){
            if(xhr.readyState == XMLHttpRequest.DONE){
                document.getElementById('prediction').innerHTML="Prediction: ₹"+xhr.responseText  }
        };
xhr.onload= function(){};
 xhr.send(fd);
    }
</script>

<!-- jQuery first, then Popper.js, then Bootstrap JS -->
<script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"
        integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj"
        crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js"
        integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo"
        crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.0/js/bootstrap.min.js"
        integrity="sha384-OgVRvuATP1z7JjHLkuOU7Xw704+h835Lr+6QL9UvYjZE3Ipu6Tp75j7Bh/kR0JKI"
        crossorigin="anonymous"></script>
</body>
</html>



//application.py:
from flask import Flask,render_template,request,redirect
from flask_cors import CORS,cross_origin
import pickle
import pandas as pd
import numpy as np

app=Flask(__name__)
cors=CORS(app)
model=pickle.load(open('LinearRegressionModel.pkl','rb'))
car=pd.read_csv(r"C:\Users\Sathwika\Cleaned_Car_data.csv")

@app.route('/',methods=['GET','POST'])
def index():
    companies=sorted(car['company'].unique())
    car_models=sorted(car['name'].unique())
    year=sorted(car['year'].unique(),reverse=True)
    fuel_type=car['fuel_type'].unique()

    companies.insert(0,'Select Company')
    return render_template('index.html',companies=companies, car_models=car_models, years=year,fuel_types=fuel_type)
@app.route('/predict',methods=['POST'])
@cross_origin()
def predict():
    company=request.form.get('company')

    car_model=request.form.get('car_models')
    year=request.form.get('year')
    fuel_type=request.form.get('fuel_type')
    driven=request.form.get('kilo_driven')
 prediction=model.predict(pd.DataFrame(columns=['name', 'company', 'year', 'kms_driven', 'fuel_type'],
                              data=np.array([car_model,company,year,driven,fuel_type]).reshape(1, 5)))
    print(prediction)
return str(np.round(prediction[0],2))
if __name__=='__main__':
    app.run()

//csvfile.py
import pandas as pd
dataset = pd.read_csv(r"C:\Users\Sathwika\Cleaned_Car_data.csv")
print(dataset)
