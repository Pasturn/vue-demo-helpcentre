<template>
        <div class="content-block-title">相关问题</div>
        <div class="list-block" id="help-list">
            <ul class="list-container">
                <li v-for="faq in faqs | filterBy keyword in 'q' 'a'">
                    <a v-link="{ path: '/detail' }" v-on:click="answer(faq.q,faq.a)" class=" item-link">
                        <div class="item-content">
                            <div class="item-inner">
                                <div class="item-title" v-html="faq.q | highLight keyword"></div>
                            </div>
                        </div>
                    </a>
                </li>
            </ul>
        </div>
    </div>
</template>
<script>
    export default {
        props:['keyword'],
        data : function (){
            return {
                'faqs' : ''
            }
        },
        ready : function(){

            // GET reques
            this.$http.get('./data/faq.json', function (data, status, request) {
                var faqData = data;
                // set data on vm
                this.$set('faqs', faqData.faqs)

            }).catch(function (data, status, request) {
                return "Data Error";
                // handle error
            })
        },
        methods : {
            answer: function (title,answer) {
                this.$dispatch('answerpage',{'title':title,'answer':answer});
            }
        }
    }
</script>