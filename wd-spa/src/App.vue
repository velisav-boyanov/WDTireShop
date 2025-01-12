<template>
    <navbar
    :pages="pages"
    :active-page="activePage"
    :nav-link-click="(index) => activePage = index">
    </navbar>

    <div v-show="false">Debug, this should not be visible</div>

    <page-viewer
    v-if="pages.length > 0"
    :page="pages[activePage]">
    </page-viewer>

    <show-list
    v-if="activePage == 0"
    :cards="cards">
    </show-list>

    <create-page
    v-if="activePage == 1"
    :page-created="pageCreated">
    </create-page>

    <find-page
    v-if="activePage == 2"
    :found-page="foundPage">
    </find-page>
</template>

<script>
import PageViewer from './components/PageViewer.vue';
import Navbar from './components/Navbar.vue';
import CreatePage from './components/CreatePage.vue';
import ShowList from './components/ShowList.vue';

    export default{
        components:
        {
            Navbar,
            PageViewer,
            CreatePage,
            ShowList
        },
        created(){
            this.getPages();
            this.getCards();
        },
        data() {
            return{
                activePage : 0,
                pages : [],
                cards : []
            };    
        },
        methods: {
            async getPages(){
                let res = await fetch('pages.json')
                let data = await res.json();

                this.pages = data;
            },
            getCards(){
                this.cards = [{
                pageTitle: "No tires added.",
                content: "Please check again later."
                }]
            },
            pageCreated(pageObj) {
                if(this.cards.length == 1 
                && this.cards[0].pageTitle.includes("No tires added."))
                {
                    this.cards.pop();  
                }
                this.cards.push(pageObj);
            }
        }
    }
</script>