## Promise Return
### Reference
[스택 오버플로우 질문](https://stackoverflow.com/questions/37533929/how-to-return-data-from-promise)

### Promise에서 resolve되어, then API로 가져온 데이터 값을 return 해서 사용하고 싶다
#### 결론 - 불가능하다
<br/>
One of the fundamental principles behind a promise is that it's handled asynchronously.<br/>
This means that you cannot create a promise and then immediately use its result synchronously in your code<br/>
(e.g. it's not possible to return the result of a promise from within the function that initiated the promise).<br/>

```javascript
function get_data(value){
  var lectures = document.getElementsByClassName("playlist__item");
  
  return axios.get(`/lecture/detail/${slug}/${value}/clicked/`)
    .then(
      function(response) {
        var data = response.data.data;
        console.log(data);
                      
        lectures[value-1].classList.add("playlist__item--clicked");

        var title_h4 = document.createElement("h4"); 
        title_h4.classList.add(`title_${value-1}`);
        var description_p = document.createElement("p");
        description_p.classList.add(`description_${value-1}`);

        var title_text = document.createTextNode(`${data.title}`);    
        var description_text = document.createTextNode(`${data.description}`);    
        title_h4.appendChild(title_text);
        description_p.appendChild(description_text);

        is_title = document.getElementsByClassName(`title_${value-1}`)[0];
        is_description = document.getElementsByClassName(`description_${value-1}`)[0];

        if (is_title && is_description) {
        } else {
            title.appendChild(title_h4);
            description.appendChild(description_p);
        }

        var step=0;

        for (step; step < lectures.length; step++ ) {
          if (step == value-1) {
            continue;
          } 

          title_step = document.getElementsByClassName(`title_${step}`)[0];
          description_step = document.getElementsByClassName(`description_${step}`)[0];

          if (title_step && description_step) {
            title_step.remove();
            description_step.remove();
          }

          console.log(step);

          if(lectures[step].classList.contains("playlist__item--clicked")){
            lectures[step].classList.remove("playlist__item--clicked");
          }
        };

        return data;
      }
    )
};
```
<br/>
나는 handleclick함수에서 리턴한 axios의 프로미스 객체에서 reponse data를 가져오고 싶었음<br/>
그래서 function 에서 `return data;` 를 했는데 비동기 행위에서 동기행위를 할 수 없다고 함.<br/>
<br/>
```javascript
function handleclick(value){
  get_data(value)
    .then(function(response){

      var lectureId = response.lectureId;
      console.log(lectureId);
      var iframe = document.getElementsByTagName("iframe")[0];
      iframe.src = `https://www.youtube.com/embed/${lectureId}?enablejsapi=1&origin=http%3A%2F%2F127.0.0.1%3A8000&widgetid=1` 
    })
}
```
그래서 걍 함수 하나 더 써서 해결함
