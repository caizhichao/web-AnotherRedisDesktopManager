<template>
  <JsonEditor ref='editor' :content='newContent' :readOnly='false'></JsonEditor>
</template>

<script type="text/javascript">
import JsonEditor from './JsonEditor';
const zlib = require('zlib');

export default {
  components: {JsonEditor},
  props: ['content'],
  computed: {
    newContent() {
      let formatStr = this.formatStr;

      if (typeof formatStr === 'string') {
        if (this.$util.isJson(formatStr)) {
          return JSON.parse(formatStr);
        }

        return formatStr;
      }

      return 'Zlib Brotli Parse Failed!';
    },
    formatStr() {
      return this.$util.zippedToString(this.content, 'brotli');
    },
  },
  methods: {
    getContent() {
      const content = this.$refs.editor.getRawContent(true);
      return zlib.brotliCompressSync(content);
    },
    copyContent() {
      return this.formatStr;
    }
  },
}
</script>
