<script>
import DiaryFirebaseManager from '../../diary/firebase/diary_firebase_manager.vue'
export default {
    name: "diary-repository",
    methods: {
        async createDiary(title, body, date, categoryId) {
            if(title === "")
                return "Please enter a title for the diary";
            if(body === "") 
                 return "Please enter content for the diary";
            if(date === null)
                return "Please choose a date for the diary";
            return await DiaryFirebaseManager.methods.createDiary(title, body, date, categoryId);
        },
        async updateDiary(id, title, body, date){
            if(title === "")
                return "Please enter a title for the diary";
            if(body === "") 
                 return "Please enter content for the diary";
            if(date === null)
                return "Please choose a date for the diary";
            return await DiaryFirebaseManager.methods.updateDiary(id,title, body, date);
        },
        async deleteDiary(diary,title,body, date) {
            if(title === "") {
                return "Diary cannot be deleted";
            }
            if(body === "") 
                 return "Diary cannot be deleted";
            if(date === null)
                return "Diary cannot be deleted";
            var response = await DiaryFirebaseManager.methods.deleteDiary(diary.id);
            return response;
        },
        async retrieveDiaries() { 
            return await DiaryFirebaseManager.methods.retrieveDiaries();
        },
        async retrieveDiariesByCategory(id) {
            if(id === "")
                return "Cannot Search by this category";
            return await DiaryFirebaseManager.methods.retrieveDiariesByCategory(id);
        },
        retrieveCategories(diaries,cat){
            if(cat === ""){
                return "There is no category defined";
                }
            let fetchedDiaries=[];
            for(let i=0;i<diaries.length;i++){
                if(diaries[i].category === cat){
                    fetchedDiaries.push(diaries[i]);
                }
            }
            return fetchedDiaries;
        },
        retrieveTitle(diaries,title){
            if(title === ""){
                return "There is no title defined";
                }
            let fetchedDiaries=[];
            for(let i=0;i<diaries.length;i++){
                if(diaries[i].title === title){
                    fetchedDiaries.push(diaries[i]);
                }
            }
         return fetchedDiaries;
        },
        async searchDiaries(title) {
            return await DiaryFirebaseManager.methods.searchDiaries(title);
        },
        sortDescending(diaries) {
            diaries.sort((diary1, diary2) => new Date(diary2.date) - new Date(diary1.date));
            return diaries;
        },
        sortAscending(diaries) {
            diaries.sort((diary1, diary2) => new Date(diary1.date) - new Date(diary2.date));
            return diaries;
        }
    }
}
</script>
