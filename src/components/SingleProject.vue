<template>
    <div class="project" :class="{ complete: project.complete }">
        <div class="actions">
            <!-- when clicked if it doesn't show, it shows and vice versa -->
            <h3 @click="showDetails = !showDetails">{{ project.title }}</h3>
            <div class="icons">
                <router-link :to="{ name: 'EditProject', params: { id: project.id }}">
                    <span class="material-symbols-outlined">edit</span>
                </router-link>
                <!-- add a click delete event to the rubbish bin -->
                <span @click="deleteProject" class="material-symbols-outlined">delete</span>
                <span @click="toggleComplete" class="material-symbols-outlined tick">done</span>
            </div>
        </div>
        <div v-if="showDetails" class="details">
            <p>{{ project.details}}</p>
            
        </div>
    </div>
  
</template>

<script>
export default {
    props: ['project'],
    // function to hide project.details on click
    data() {
        return {
            // only if this is true, details will show
            showDetails: false,

            // end point to delete an item (db of projects)
            uri :'http://localhost:3000/projects/' + this.project.id
        }
    },
    
    methods: {
        // delete project method
    // send delete message to json server to delete an item from the projects
        deleteProject() {
            fetch(this.uri, { method: 'DELETE'})
                .then(() => this.$emit('delete', this.project.id))
                .catch(err => console.log(err))
        },
        // update the done icon (project done and undone)
        toggleComplete() {
            fetch(this.uri, {
                method: 'PATCH',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify({ complete: !this.project.complete })
            })  .then(() => {
                this.$emit('complete', this.project.id)
            })  .catch((err) => console.log(err))
        }
    }


}
</script>

<style>
/* projects design */
.project {
    margin: 20px auto;
    background: white;
    padding: 10px 20px;
    border-radius: 4px;
    box-shadow: 1px 2px 3px rgba(0,0,0,0.05);
    border-left: 5px solid #e4a444;
}
h3 {
    cursor: pointer;
}

.actions {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.material-symbols-outlined {
    font-size: 24px;
    margin-left: 10px;
    color: #bbb;
    cursor: pointer;
}

.material-symbols-outlined:hover {
    color: #777;
}

/* completed project design */
.project.complete {
    border-left: 5px solid #1dc72b;
}
.project.complete .tick {
    color: #1dc72b;
}
</style>