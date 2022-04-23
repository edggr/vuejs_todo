<template>
  <div class='ui basic content center aligned segment'>
    <button class='ui basic excel_download btn' @click="downloadTodos('csv')" >
      Download in Excel
      <i class='download icon'></i>
    </button>
    <button class='ui basic json_download btn' @click="downloadTodos('json')" >
      Download in JSON
      <i class='download icon'></i>
    </button>
  </div>
</template>

<script>

export default {
  props: {
    localstorageTodos: {
      type: Array,
      required: true,
    },
  },
  methods: {
    downloadTodos(fileFormat) {
      if (fileFormat === 'csv') {
        let tmp = JSON.stringify(Object.values(this.localstorageTodos));
        tmp = tmp.substring(2);
        tmp = tmp.slice(0, -2);
        tmp = tmp.replace(/"uid":/g, '');
        tmp = tmp.replace(/"title":/g, '');
        tmp = tmp.replace(/"project":/g, '');
        tmp = tmp.replace(/"responsible":/g, '');
        tmp = tmp.replace(/"done":/g, '');
        tmp = tmp.replace(/},{/g, '\n');
        tmp = tmp.replace(/,/g, ';');
        tmp = tmp.replace(/"/g, '');
        tmp = tmp.replace(/true/g, '"YES"');
        tmp = tmp.replace(/false/g, '"NO"');
        const todosToDownload = `ID;TITLE;PROJECT;RESPONSIBLE;DONE\n ${tmp}`;
        const dataStr = `data:application/vnd.ms-excel;charset=utf-8,${encodeURIComponent(todosToDownload)}`;
        const downloadAnchorNode = document.createElement('a');
        downloadAnchorNode.setAttribute('href', dataStr);
        downloadAnchorNode.setAttribute('download', 'todos.csv');
        document.body.appendChild(downloadAnchorNode);
        downloadAnchorNode.click();
        downloadAnchorNode.remove();
      }
      if (fileFormat === 'json') {
        const dataStr = `data:text/plain;charset=utf-8,${encodeURIComponent(JSON.stringify(this.localstorageTodos))}`;
        const downloadAnchorNode = document.createElement('a');
        downloadAnchorNode.setAttribute('href', dataStr);
        downloadAnchorNode.setAttribute('download', 'todos.json');
        document.body.appendChild(downloadAnchorNode);
        downloadAnchorNode.click();
        downloadAnchorNode.remove();
      }
    },
  },
};
</script>
