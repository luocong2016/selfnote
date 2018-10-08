```
/*
  // 验证时间格式
  params: yyyy-MM-dd | yyyy:MM:dd | yyyy/MM/dd | yyyy-M-D | yyyy:M:D | yyyy/M/D
  return: boolean
*/
function proofTimeFunc(drq) {
    var date = drq || '';
    var result = date.match(/^(\d{1,4})(-|\/|:)(\d{1,2})\2(\d{1,2})$/);
    
    if (result == null) { return false; }
   
    var d = new Date(result[1], result[3] - 1, result[4]);
    
    return (d.getFullYear() == result[1] && (d.getMonth() + 1) == result[3] && d.getDate() == result[4]);
}
```