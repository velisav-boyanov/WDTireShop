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
    :search-for-page="searchForPage">
    </find-page>

    <show-list
    v-if="activePage == 2"
    :cards="foundPages">
    </show-list> 

    <div 
    v-if="activePage == 0"
    align="center" 
    class="cus-container">
        <h2 class="cus">
            Our custom selection:
        </h2>
    </div>

    <show-list
    v-if="activePage == 0"
    :cards="old">
    </show-list>

</template>

<script>
import PageViewer from './components/PageViewer.vue';
import Navbar from './components/Navbar.vue';
import CreatePage from './components/CreatePage.vue';
import ShowList from './components/ShowList.vue';
import FindPage from './components/FindPage.vue';

    export default{
        components:
        {
            Navbar,
            PageViewer,
            CreatePage,
            ShowList,
            FindPage
        },
        created(){
            this.getPages();
            this.getCards();
            this.getOldCards();
        },
        data() {
            return{
                activePage : 0,
                pages : [],
                cards : [],
                foundPages : [],
                old : []
            };    
        },
        methods: {
            async getPages(){
                let res = await fetch('pages.json')
                let data = await res.json();

                this.pages = data;
            },
            async getOldCards(){
                let res = await fetch('cards.json')
                let data = await res.json();

                this.old = data;
            },
            getCards(){
                this.cards = [{
                pageTitle: "No customer tires added.",
                content: "Please check again later."
                }]
            },
            pageCreated(pageObj) {
                if(this.cards.length > 0 
                && this.cards.filter(card => card.content.includes("No customer tires added.")))
                {
                    this.cards.pop();  
                }
                this.cards.push(pageObj);
            },
            searchForPage(searchObj) {
                this.foundPages = this.cards.filter(card => card.pageTitle.includes(searchObj.searchKey));
                if(searchObj.contentToo){
                    if(this.foundPages.length == 0){
                        this.foundPages = this.cards.filter(card => card.content.includes(searchObj.searchKey));   
                    }else{
                        this.foundPages.concat(this.cards.filter(card => card.content.includes(searchObj.searchKey)));
                    }
                }
            }
        }
    }
</script>

<style :scoped>
h2.cus {
    margin-top: 10px;
    color: blue;
}
</style>