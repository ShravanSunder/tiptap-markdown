<template>
    <div class="min-vh-100 p-4">
        <div class="row h-100">
            <div class="col-md-6">
                <textarea class="form-control h-100" rows="10" ref="markdown" v-model="markdown" @input="handleInput"></textarea>
            </div>
            <div class="col-md-6">
                <div class="editor card h-100">
                    <div class="card-header sticky-top bg-white">
                        <MenuBar :editor="editor" />
                    </div>

                    <EditorContent class="editor__content flex-fill" style="margin: -1px" :editor="editor" />
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import { Editor, EditorContent } from '@tiptap/vue-3';
    import StarterKit from '@tiptap/starter-kit';
    import Table from '@tiptap/extension-table';
    import TableRow from '@tiptap/extension-table-row';
    import TableCell from '@tiptap/extension-table-cell';
    import TableHeader from '@tiptap/extension-table-header';
    import Underline from '@tiptap/extension-underline';
    import Image from '@tiptap/extension-image';
    import Link from '@tiptap/extension-link';
    import TaskList from '@tiptap/extension-task-list'
    import TaskItem from '@tiptap/extension-task-item'
    import CodeBlock from "@tiptap/extension-code-block";
    import { Markdown } from "tiptap-markdown";
    import MenuBar from "./MenuBar.vue";
    import content from '../content.md?raw';
    import Highlight from "../extensions/highlight";
    import Container from "../extensions/container";

    export default {
        components: {
            EditorContent,
            MenuBar,
        },
        data() {
            return {
                editor: null,
                markdown: null,
            }
        },
        methods: {
            updateMarkdownOutput() {
                console.log(this.editor.storage.markdown);
                this.markdown = this.editor.storage.markdown.getMarkdown();
            },
            handleInput() {
                this.editor.commands.setContent(this.markdown);
            },
        },
        mounted() {
            this.editor = new Editor({
                extensions: [
                    Markdown.configure({
                    }),
                    StarterKit.configure({
                        codeBlock: false,
                    }),
                    Table.configure({
                        resizable: false,
                    }),
                    TableRow,
                    TableHeader,
                    TableCell,
                    Underline,
                    Image.configure({
                        inline: true,
                    }),
                    Link,
                    Highlight,
                    TaskList,
                    TaskItem,
                    CodeBlock.configure({
                        languageClassPrefix: 'lang-',
                    }),
                    Container,
                ],
                content,
                onUpdate: () => {
                    this.updateMarkdownOutput();
                },
                editorProps: {
                    attributes: {
                        class: 'card-body form-control h-100',
                    }
                }
            });
            this.updateMarkdownOutput();
        },
    }
</script>
