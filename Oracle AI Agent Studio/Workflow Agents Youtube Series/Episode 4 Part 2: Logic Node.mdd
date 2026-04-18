**Pagination Array based on offsets- Code Node**

let total = $context.$nodes.FETCH_INVOICE.$output.totalResults;
const fetchSize = $context.$variables.fetchsize; // e.g., 10
let offsets = [];
for (let i = 0; i < total; i += fetchSize) {
 offsets.push({ offset: i });
}
return offsets;

**Combine multiple arrays into One- Code Node**

const resultsArray = $context.$nodes.LOOPING.$output ;
const returnArray = []; 
resultsArray.forEach(item => {
  const jsonResult = JSON.parse(item);
  returnArray.push(...jsonResult.items);
});
return returnArray;
