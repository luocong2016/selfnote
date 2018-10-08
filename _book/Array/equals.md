```
function findArray (array, feature, all) {
  all = all || true;
  for (var index in array) {
    var cur = array[index];
    if (feature instanceof Object) {
      var allRight = true;
      for (var key in feature) {
        var value = feature[key];
        if (cur[key] == value && !all) return index;
        if (all && cur[key] != value) {
          allRight = false;
          break;
        }
      }
      if (allRight) return index;
    } else {
      if (cur == feature) {
        return index;
      }
    }
  }
  return -1;
}
```