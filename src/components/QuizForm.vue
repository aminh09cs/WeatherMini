<template>
    <form class="screen">
        <div class="container">
            <div class="row">
                <h1 class="title">Now, about your project...</h1>
            </div>
            <div class="row">
                <div class="col-7">
                    <div class="section">
                        <p>
                            We like being on a first-name basis, but it also helps us get in
                            touch with you.
                        </p>
                        <div class="form-group">
                            <div class="form-item col">
                                <label for="name">What is your full name?</label>
                                <input id="name" type="text" placeholder="John Smith" v-model="quiz.fullName" />
                                {{ quiz.fullName }}
                            </div>
                            <div class="form-item col">
                                <label for="email">What is your email address?</label>
                                <input id="email" type="email" placeholder="skyalbert.960@gmail.com"
                                    v-model="quiz.email" />
                                {{ quiz.email }}
                            </div>
                        </div>
                    </div>
                    <div class="section options">
                        <p>
                            What sort of creative work do you need help with? You can read
                            about our services
                        </p>
                        <div class="row">
                            <div class="col-4" v-for="option in jobOptions" :key="option.id">
                                <div class="block">
                                    <label class="option">
                                        <span>{{ option.name }}</span>
                                        <input type="checkbox" />
                                        <span class="checkmark"></span>
                                    </label>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-4">
                    <div class="section right">
                        <p>
                            Tell us what you need help with, the purpose of this project and
                            problems wr're solving.
                        </p>
                        <div class="form-item">
                            <label for="about">What is your project all about?</label>
                            <textarea id="about"
                                placeholder="Hey RHP Team, I’d love  to talk to you about  branding this Something AI  project we’re working..."
                                v-model="quiz.desc" />
                            {{ quiz.desc }}
                        </div>
                        <div class="form-item">
                            <ul class="items">
                                <li class="item">
                                    <div class="file">
                                        <div class="file-icon">
                                            <img src="../assets/images/file.png" alt="File Icon" />
                                        </div>
                                        <div class="file-name">
                                            website-information-architecture.pdf
                                        </div>
                                        <div class="file-icon">
                                            <img src="../assets/images/close.png" alt="Close Icon" />
                                        </div>
                                    </div>
                                </li>
                                <li class="item">
                                    <div class="file">
                                        <div class="file-icon">
                                            <img src="../assets/images/file.png" alt="File Icon" />
                                        </div>
                                        <div class="file-name">logo-animation.mov</div>
                                        <div class="file-icon">
                                            <img src="../assets/images/close.png" alt="Close Icon" />
                                        </div>
                                    </div>
                                </li>
                                <li class="item">
                                    <div class="file">
                                        <div class="file-icon">
                                            <img src="../assets/images/upload.png" alt="Upload Icon" />
                                        </div>
                                        <div class="file-name">Attach files</div>
                                    </div>
                                </li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="action">
            <div class="container">
                <div class="remaining">3/6 questions remaining</div>
                <div class="progress">
                    <div class="progress-inner">
                        <div class="progress-mask"></div>
                    </div>
                </div>
                <button type="submit" class="btn btn-submit">Submit</button>
            </div>
        </div>
    </form>
</template>
<script setup>
import { ref } from 'vue';
const jobOptions = [
    {
        id: 1,
        name: "Branding",
    },
    {
        id: 2,
        name: "Strategy",
    },
    {
        id: 3,
        name: "Motion Design",
    }
]

let quiz = ref({
    fullName: "",
    email: "",
    desc: "",
})
</script>
<style>
.container {
    display: flex;
    align-items: center;
    justify-content: flex-start;
    flex-direction: column;
    width: 70%;
    margin: 0 auto;
    padding: 0 1rem;
}

.row {
    display: flex;
    flex-wrap: wrap;
}

.col {
    flex: 1 0 0%;
}

.col-7 {
    flex: 0 0 auto;
    width: calc(100% / 12 * 7);
}

.col-4 {
    flex: 0 0 auto;
    width: calc((100% - 3rem) / 12 * 4);
}

.btn {
    display: inline-block;
    font-weight: 500;
    color: #212529;
    text-align: center;
    text-decoration: none;
    vertical-align: middle;
    cursor: pointer;
    background-color: transparent;
    border: 1px solid transparent;
    padding: 0.75rem 1.5rem;
    font-size: 1rem;
    border-radius: 0.25rem;
    transition: color 0.15s ease-in-out, background-color 0.15s ease-in-out,
        border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}

.btn:hover {
    color: #fff;
    background-color: #157347;
    border-color: #146c43;
}

.btn-submit {
    color: #fff;
    background-color: #198754;
    border-color: #198754;
}

/* Details */

.action {
    border-top: 1px solid var(--secondary);
    background-color: var(--dark);
    position: fixed;
    z-index: 10;
    height: 100px;
    display: flex;
    align-items: center;
    bottom: 0;
    left: 0;
    width: 100%;
}

.action .container {
    flex-direction: row;
    align-items: center !important;
    justify-content: space-between;
}

.action .progress {
    height: 8px;
    width: 40%;
}

.progress .progress-inner {
    background-color: var(--secondary);
    border-radius: 8px;
    height: 100%;
    width: 100%;
}

.progress .progress-mask {
    background-color: var(--primary);
    border-radius: 8px;
    height: 100%;
    width: 50%;
}

.screen .container {
    align-items: flex-start;
}

.screen .container>.row:nth-child(2) {
    width: 100%;
}

.screen .title {
    margin-top: 6.625rem;
    font-size: 56px;
    font-weight: bold;
}

.screen .section {
    margin-top: 3.375rem;
}

.screen .row:last-child {
    justify-content: space-between;
}

.section .form-group {
    margin-top: 1.625rem;
    display: flex;
    margin-left: -0.5rem;
    margin-right: -0.5rem;
}

.form-group .col {
    margin: 0 0.5rem;
}

.section.right .form-item {
    margin-top: 2rem;
}

.section.right .form-item:last-child {
    margin-top: 1.5rem;
}

.form-item label {
    width: 100%;
    display: block;
    margin-bottom: 1rem;
    font-weight: 600;
}

.form-item textarea {
    width: 100%;
    height: 168px;
    border-radius: 0.25rem;
    background-color: var(--secondary);
    resize: none;
    border: 0;
    padding: 1rem;
    color: Var(--light);
    font-family: var(--font);
    font-size: 1rem;
}

.form-item input {
    padding: 1rem;
    background-color: var(--secondary);
    width: 100%;
    border: 1px solid var(--secondary);
    border-radius: 0.25rem;
    color: Var(--light);
    font-family: var(--font);
    font-size: 1rem;
}

.error-text {
    margin-top: 0.25rem;
    color: var(--error);
}

.success-text {
    margin-top: 0.25rem;
    color: var(--primary);
}

.form-item input.error {
    border-color: var(--error);
}

.form-item input.success {
    border-color: var(--primary);
}

.form-item .items {
    list-style: none;
    margin: 0;
}

.form-item .items .item {
    margin-bottom: 1rem;
}

.form-item .items .item:last-child {
    cursor: pointer;
    margin-top: 2rem;
    color: var(--primary);
}

.file {
    display: flex;
    align-items: center;
}

.file .file-icon {
    display: flex;
    align-items: center;
}

.file .file-icon:last-child {
    cursor: pointer;
    margin-left: auto;
}

.file .file-name {
    font-weight: 600;
    margin-left: 0.75rem;
}

.items .item:last-child .file-name {
    font-weight: 600;
}

.option {
    display: block;
    position: relative;
    cursor: pointer;
    font-size: 1.125rem;
    user-select: none;
}

.option span:first-child {
    padding-left: 2.25rem;
}

.option input {
    position: absolute;
    opacity: 0;
    cursor: pointer;
}

.option .checkmark {
    position: absolute;
    top: 0;
    left: 0;
    height: 24px;
    width: 24px;
    background-color: transparent;
    border-radius: 50%;
    border: 1px solid var(--third);
}

.block:hover {
    background-color: var(--primary);
}

.block:hover input~.checkmark {
    background-color: var(--light);
}

.block input:checked~.checkmark {
    background-color: var(--light);
}

.checkmark:after {
    content: "";
    position: absolute;
    display: none;
}

.block input:checked~.checkmark:after {
    display: block;
}

.block input:checked~.block {
    background-color: var(--primary);
}

.block .checkmark:after {
    top: 7px;
    left: 6px;
    width: 10px;
    height: 10px;
    background: url("../assets/images/check.png") no-repeat center center;
    background-size: contain;
}

.block {
    padding: 1rem;
    cursor: pointer;
    background-color: var(--secondary);
    border-radius: 0.25rem;
}

.section.options p {
    margin-bottom: 1.5rem;
}

.section.options .row {
    margin: 0 -0.5rem;
    justify-content: flex-start;
}

.section.options .col-4 {
    margin: 0 0.5rem;
    margin-bottom: 1rem;
}

.option span {
    font-size: 13px;
}

@media (min-width: 1440px) {
    .container {
        max-width: 1360px;
    }
}
</style>