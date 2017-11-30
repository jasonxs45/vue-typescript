# vue-typescript
用vue-cli生成基本之后，改成配合使用typescrpit的结构
改动内容有：
#1.webpack.conf.base.js
  1.1 入口文件修改为main.ts
    entry: {
      app: './src/main.ts'
    }
  1.2 解析文件类型添加.ts,.tsx
    resolve: {
      extensions: ['.ts','.tsx','.js', '.vue', '.json'],
      alias: {
        'vue$': 'vue/dist/vue.esm.js',
        '@': resolve('src'),
      }
    }
  1.3 添加ts-loader
    {
      test: /\.tsx?$/,
      loader: 'ts-loader',
      exclude: /node_modules/,
      options: {
        appendTsSuffixTo: [/\.vue$/],
      }
    }
 

2.在项目根目录文件夹下面添加ts的配置文件tsconfig.json
{
 # "compilerOptions": {
    "strict": true,
    "module": "es2015",
    "moduleResolution": "node",
    "target": "es5",
    "allowSyntheticDefaultImports": true,
    "experimentalDecorators": true,
    "lib": [
      "es2017",
      "dom"
    ]
  }
}

3.在src文件夹下面新建一个vue-shims.d.ts文件，用于ts识别Vue单文件
    declare module "*.vue" {
      import Vue from "vue";
      export default Vue;
    }
 
4.安装typescript,ts-loader,vue-class-component,vue-property-decorator四个npm包
  4.1 typescript,ts-loader用于支持typescript语法
  4.2 vue-class-component,vue-property-decorator用于方便将.vue文件修改成声明组件时基于类的API风格，
      其中vue-property-decorator是依赖vue-class-component的

5.一些注意的点
  5.1 单文件中script标签需要加上 lang="ts"
  5.2 在引入自定义组件的时候需要加上.vue的后缀 import App from './App.vue'
  5.3 使用vue-property-decorator之后大概是这么写
    <script lang="ts">
    import { Vue, Component, Prop, Emit } from "vue-property-decorator";
    @Component
    export default class Hello extends Vue {
      @Prop({
        type: String,
        default: "Default Value"
      })
      name: string;
      @Prop({
        type: Number,
        default: 0
      })
      initialEnthusiasm: number;
      enthusiasm = this.initialEnthusiasm;
      @Emit("plus")
      increment() {
        this.enthusiasm++;
        console.log(this.enthusiasm);
      }
      @Emit("minus")
      decrement() {
        if (this.enthusiasm > 0) {
          this.enthusiasm--;
        }
        console.log(this.enthusiasm);
      }
      // 计算属性
      get exclamationMarks(): string {
        return Array(this.enthusiasm + 1).join("!");
      }
    }
    </script>
    @Prop 对应之前的props
    @Emit 对应派送事件
