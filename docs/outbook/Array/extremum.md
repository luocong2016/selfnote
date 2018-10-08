```
var array = [
  {
    "index_id": 119,
    "area_id": "18335623",
    "name": "满意度",
    "value": "100"
  },
  {
    "index_id": 119,
    "area_id": "18335624",
    "name": "满意度",
    "value": "20"
  },
  {
    "index_id": 119,
    "area_id": "18335625",
    "name": "满意度",
    "value": "80"
  }
];

/**
 * 
 * @param {*} arr array
 * @param {*} attr object[property]
 * @param {*} sort -1 最小值, 其他默认：最大值
 */
function getObjArrFunc (arr, attr, sort) {
  sort = sort === -1 ? 'min' : 'max';
  return Math[sort].apply(Math, arr.map(function (o) { return o[attr] }));
}

var max = getObjArrFunc (array, 'value');
var min = getObjArrFunc (array, 'value', -1);

console.log(max, min);
```
## or
```
if (!Array.prototype.extremum) {
  Array.prototype.extremum = function(attr, sort) {
    sort = sort === -1 ? 'min' : 'max';
    return Math[sort].apply(Math, this.map(function (o) { return o[attr] }));
  }
}

var max = array.extremum('value');
var min = array.extremum('value', -1);
```