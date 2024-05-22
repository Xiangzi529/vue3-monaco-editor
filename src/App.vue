<template>
  <div class="container" ref="container"></div>
</template>

<script setup lang="ts">
import * as monaco from "monaco-editor";
import { onMounted, ref } from "vue";
const codes = `
import { PropType, ComputedRef, ExtractPropTypes, Ref } from 'vue';

export interface OptionObjectItem {
  name: string;
  value: string | number;
  _checked: boolean;
  [key: string]: unknown;
}

export type OptionItem = number | string | ({ value: string | number } & Partial<OptionObjectItem>);
export type Options = Array<OptionItem>;

export type ModelValue = number | string | Array<number | string>;
export type filterValue = boolean | ((query: string) => void);
export type SelectSize = 'sm' | 'md' | 'lg';

export type Placement =
  | 'top'
  | 'right'
  | 'bottom'
  | 'left'
  | 'top-start'
  | 'top-end'
  | 'right-start'
  | 'right-end'
  | 'bottom-start'
  | 'bottom-end'
  | 'left-start'
  | 'left-end';
export const selectProps = {
  modelValue: {
    type: [String, Number, Array] as PropType<ModelValue>,
    default: '',
  },
  'onUpdate:modelValue': {
    type: Function as PropType<(val: ModelValue) => void>,
    default: undefined,
  },
  options: {
    type: Array as PropType<Options>,
    default: () => [],
  },
  position: {
    type: Array as PropType<Placement[]>,
    default: () => ['bottom', 'top', 'left', 'right'],
  },
  size: {
    type: String as PropType<SelectSize>,
<<<<<<< HEAD
    default: '',
=======
    default: 'md',
>>>>>>> other-branch
  },
  // TODO: 这个api命名不合理
  overview: {
    type: String as PropType<'border' | 'underlined'>,
    default: 'border',
  },
  placeholder: {
    type: String,
    default: '',
  },
  multiple: {
    type: Boolean,
    default: false,
  },
  disabled: {
    type: Boolean,
    default: false,
  },
  allowClear: {
    type: Boolean,
    default: false,
  },
  optionDisabledKey: {
    type: String,
    default: '',
  },
  collapseTags: {
    type: Boolean,
    default: false,
  },
  collapseTagsTooltip: {
    type: Boolean,
    default: false,
  },
  filter: {
    type: [Boolean, Function] as PropType<filterValue>,
    default: false,
  },
  remote: {
    type: Boolean,
    default: false,
  },
  allowCreate: {
    type: Boolean,
    default: false,
  },
  noDataText: {
    type: String,
    default: '',
  },
  noMatchText: {
    type: String,
    default: '',
  },
  loading: {
    type: Boolean,
    default: false,
  },
  loadingText: {
    type: String,
    default: '',
  },
  onToggleChange: {
    type: Function as PropType<(bool: boolean) => void>,
    default: undefined,
  },
  onValueChange: {
    type: Function as PropType<(item: OptionItem, index: number) => void>,
    default: undefined,
  },
  multipleLimit: {
    type: Number,
    default: 0,
  },
  showGlowStyle: {
    type: Boolean,
    default: true,
  },
  menuClass: {
    type: String,
    default: '',
  },
  maxLength: {
    type: Number,
  },
} as const;

export type SelectProps = ExtractPropTypes<typeof selectProps>;

export type OptionModelValue = number | string;

export interface SelectContext extends SelectProps {
  selectDisabled: boolean;
  selectSize: string;
  isOpen: boolean;
  selectedOptions: OptionObjectItem[];
  filterQuery: string;
  valueChange: (item: OptionObjectItem) => void;
  handleClear: () => void;
  updateInjectOptions: (item: Record<string, unknown>, operation: string, isObject: boolean) => void;
  tagDelete: (data: OptionObjectItem) => void;
  onFocus: (e: FocusEvent) => void;
  onBlur: (e: FocusEvent) => void;
  debounceQueryFilter: (query: string) => void;
}

export const optionProps = {
  value: {
    type: [String, Number] as PropType<OptionModelValue>,
    default: '',
  },
  name: {
    type: String,
    default: '',
  },
  disabled: {
    type: Boolean,
    default: false,
  },
  create: {
    type: Boolean,
    default: false,
  },
};

export type OptionProps = ExtractPropTypes<typeof optionProps>;

export interface UseOptionReturnType {
  currentName: ComputedRef<OptionModelValue>;
  selectOptionCls: ComputedRef<string>;
  isVisible: ComputedRef<boolean>;
  optionSelect: () => void;
}

export interface UseSelectFunctionReturn {
  isSelectFocus: Ref<boolean>;
  blur: () => void;
  focus: () => void;
}

export const optionGroupProps = {
  label: {
    type: String,
  },
  disabled: {
    type: Boolean,
    default: false,
  },
};

export type OptionGroupProps = ExtractPropTypes<typeof optionGroupProps>;

export type OptionGroupContext = OptionGroupProps;
`
const container = ref<HTMLElement | null>(null);
let mergeEditor = ref<monaco.editor.IStandaloneCodeEditor>();

const createCodeLen = (
  id: string,
  title: string,
  startLine: number,
  endLine: number,
  column: number,
  editor: monaco.editor.IStandaloneCodeEditor,
  replaceValue: string
) => {
  return {
    range: {
      startLineNumber: startLine,
      startColumn: column,
      endLineNumber: endLine,
      endColumn: column
    },
    id,
    command: {
      id: "mergeVersion",
      title,
      arguments: [
        {
          startLine,
          endLine,
          editor,
          replaceValue
        }
      ]
    },
    options: {
      isWholeLine: true,
      className: "myDecoration" // 应用于文本的 CSS 类
    }
  };
};

const createMergeDecoration = (startLine: number, endLine: number, classStyleName: string) => {
  console.log(startLine, endLine, classStyleName);
  return {
    range: new monaco.Range(startLine, 1, endLine, 1),
    options: {
      isWholeLine: true,
      className: classStyleName
    }
  };
};


onMounted(() => {

  // init
  mergeEditor.value = monaco.editor.create(container.value!, {
    value: codes, //编辑器的值
    language: "javascript",
    contextmenu: false,
    theme: 'vs-dark', //vs, hc-black, or vs-dark
    readOnly: false,
    minimap: {
      enabled: true //是否开启小地图
    },
    lineHeight: 20, //行高
    foldingStrategy: "indentation", //折叠策略
    scrollBeyondLastLine: false,
    renderWhitespace: "all",
    overviewRulerBorder: false, // 滚动条边框
    automaticLayout: true
  });

  monaco.editor.registerCommand("mergeVersion", (ctx, args) => {
    let { editor, replaceValue, startLine, endLine } = args;
    let content = editor.getValue();
    console.log(content);
    let contentSplit = content.split("\n");

    let newContent = "";
    let start = false;
    contentSplit.forEach((line: string, index: number) => {
      let currentLine = index + 1;
      if (currentLine === startLine) {
        start = true;
      } else if (currentLine === endLine) {
        start = false;
        newContent = newContent + replaceValue;
        return;
      }
      if (start === false) {
        newContent = newContent + "\n" + line;
      }
    });
    editor.executeEdits("replace", [
      {
        identifier: "insert all",
        range: new monaco.Range(startLine, 1, endLine, 1000),
        text: replaceValue,
        forceMoveMarkers: true
      }
    ]);
  });

  type CompareData = {
    startLine: number;
    endLine: number;
    currentVersionContent: string;
    currentVersionLineCount: number;
    incomingVersionContent: string;
    incomingVersionLineCount: number;
  };

  monaco.languages.registerCodeLensProvider("*", {
    provideCodeLenses: model => {
      let newCodeLens = [];
      let editor = mergeEditor.value;
      if (editor) {
        console.log(11);
        let content = model.getValue();
        let contentSplit = content.split("\n");
        let compareDatas: CompareData[] = [];

        //当前的所在的状态，start开始状态，split分层状态，end结束状态
        let status = "end";
        //当前版本的内容
        let versionContent = "";
        //冲突块
        let conflitBlock: CompareData = {
          startLine: -1,
          endLine: -1,
          currentVersionContent: "",
          currentVersionLineCount: -1,
          incomingVersionContent: "",
          incomingVersionLineCount: -1
        };
        let versionContentCount = 0;
        contentSplit.forEach((line, index) => {
          // console.log(line)
          let currentLine = index + 1;
          if (line.startsWith("<<<<<<<")) {
            conflitBlock = {
              //信息提示的行号
              startLine: currentLine,
              //结束行
              endLine: -1,
              //本地版本内容
              currentVersionContent: "",
              //当前版本的总行数
              currentVersionLineCount: -1,
              //远程版本内容
              incomingVersionContent: "",
              //远程版本的总行数
              incomingVersionLineCount: -1
            };
            status = "start";
            versionContent = "";
          } else if (line.startsWith(">>>>>>>") && status === "split") {
            conflitBlock.incomingVersionContent = versionContent;
            conflitBlock.endLine = currentLine;
            conflitBlock.incomingVersionLineCount = versionContentCount;
            versionContent = "";
            versionContentCount = 0;
            status = "end";
            compareDatas.push(conflitBlock);
          } else if (line.startsWith("=======") && status === "start") {
            conflitBlock.currentVersionContent = versionContent;
            conflitBlock.currentVersionLineCount = versionContentCount;
            versionContent = "";
            versionContentCount = 0;
            status = "split";
          } else {
            if (status === "start" || status === "split") {
              versionContent = versionContent + line;
              versionContentCount = versionContentCount + 1;
            }
          }
        });

        let codeDecorations = [];
        console.log(compareDatas);
        if (compareDatas.length > 0) {
          for (let compareData of compareDatas) {
            let {
              startLine,
              endLine,
              currentVersionContent,
              currentVersionLineCount,
              incomingVersionContent,
              incomingVersionLineCount
            } = compareData;

            //使用本地版本触发按钮
            newCodeLens.push(
              createCodeLen("currentVersion", "使用本地的版本", startLine, endLine, 1, editor, currentVersionContent)
            );

            //使用远程版本按钮
            newCodeLens.push(
              createCodeLen("incomingVersion", "使用远程版本", startLine, endLine, 2, editor, incomingVersionContent)
            );

            //合并两个版本按钮
            newCodeLens.push(
              createCodeLen(
                "bothVersion",
                "同时使用两个版本",
                startLine,
                endLine,
                3,
                editor,
                currentVersionContent + "\r\n" + incomingVersionContent
              )
            );
            //当前版本的标签
            codeDecorations.push(createMergeDecoration(startLine, startLine, "currentVersionTag"));
            //当前版本的代码范围 开始行数 + 当前版本的总行数
            codeDecorations.push(createMergeDecoration(startLine, startLine + currentVersionLineCount, "currentContentTag"));

            //远程版本的代码范围 结束行数 - 远程行数的总行数
            codeDecorations.push(createMergeDecoration(endLine - 1, endLine - incomingVersionLineCount, "incomingContentTag"));
            //远程版本的标签
            codeDecorations.push(createMergeDecoration(endLine, endLine, "incomingVersionTag"));
          }
        }
        // editor.deltaDecorations([], codeDecorations);
        // editor.createDecorationsCollection(codeDecorations)
      }

      return {
        lenses: newCodeLens,
        dispose: () => {}
      };
    },
    resolveCodeLens: function (model, codeLens) {
      return codeLens;
    }
  });

});
</script>

<style scoped lang="scss">

.container {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  background-color: #f40;
}
</style>
