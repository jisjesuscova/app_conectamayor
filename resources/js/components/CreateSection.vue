<template>
    <div>
        <!-- Begin Page Content -->
        <div class="container-fluid">
            <h1 class="h3 mb-2 text-gray-800">
                Crear Sección
            </h1>
            <!-- DataTales Example -->
            <div class="card shadow mb-4">
                <div class="card-header py-3">
                    <div class="row">
                        <div class="col-sm-6">
                            <h6 class="m-0 font-weight-bold text-primary">Información</h6>
                        </div>
                        <div class="col-sm-6">
                            <h6 class="m-0 text-danger float-right">* Campos Obligatorios</h6>
                        </div>
                    </div>
                </div>
                <div class="card-body">
                    <div class="table-responsive">
                        <div v-if="loading">
                            <center>
                                <clip-loader :color="color"></clip-loader>
                            </center>
                        </div>
                        <div v-else>
                            <form @submit.prevent="onSubmit" ref="createBill" enctype="multipart/form-data">
                                <div v-if="errors.length" class="alert alert-danger" role="alert">
                                    <b>Por favor, corrija los siguientes errores:</b>
                                    <ul>
                                        <li v-for="error in errors">{{ error }}</li>
                                    </ul>
                                </div>
                                <div class="form-group row">
                                    <div class="col-sm-4">
                                        <label for="exampleInputEmail1">Título <h6 class="m-0 text-danger float-right">*</h6></label>
                                        <input
                                        type="text" 
                                        v-model="form.title" 
                                        maxlength="36"
                                        class="form-control"
                                        placeholder="Ingresa el título"
                                        >
                                        <span class="col-sm-12">{{charactersLeft}}</span>
                                    </div>
                                    <div class="col-sm-4">
                                        <label for="exampleInputEmail1">Sub-título</label>
                                        <input
                                        type="text" 
                                        v-model="form.subtitle" 
                                        maxlength="30"
                                        class="form-control"
                                        placeholder="Ingresa el subtítulo"
                                        >
                                    </div>
                                    <div class="col-sm-4">
                                        <label for="exampleInputEmail1">Google Tag <h6 class="m-0 text-danger float-right">*</h6></label>
                                        <input
                                        type="text" 
                                        v-model="form.google_tag" 
                                        class="form-control"
                                        placeholder="Ingresa el google tag"
                                        >
                                    </div>
                                </div>
                                <div class="form-group row">
                                    <div class="col-sm-6">
                                        <label for="exampleInputEmail1">Color <h6 class="m-0 text-danger float-right">*</h6></label>
                                        <div class="form-group row">
                                            <div class="col-sm-2">
                                                <v-input-colorpicker v-model="color" @change="handleChange" />
                                            </div>
                                            <div class="col-sm-10">
                                                <input
                                                type="text" 
                                                v-model="form.color" 
                                                class="form-control"
                                                placeholder="Ingresa el color"
                                                >
                                            </div>
                                        </div>
                                    </div>
                                    <div class="col-sm-6">
                                        <label for="exampleInputEmail1">Posición <h6 class="m-0 text-danger float-right">*</h6></label>
                                        <input
                                        type="number" 
                                        v-model="form.position" 
                                        min="0"
                                        class="form-control"
                                        placeholder="Ingresa la posición"
                                        >
                                    </div>
                                </div>
                                <div class="form-group row">
                                    <div class="col-sm-6">
                                        <label for="exampleInputEmail1">Tipo de Icono <h6 class="m-0 text-danger float-right">*</h6></label>
                                        <select class="form-control" id="exampleFormControlSelect1"
                                        v-model="form.icon_type_id"
                                        >
                                            <option :value="null">Seleccionar</option>
                                            <option :value="2">Fa Icon</option>
                                            <option :value="3">Ionic Icon</option>
                                            <option :value="1">Imagen</option>
                                        </select>
                                    </div>
                                    <div class="col-sm-6" v-if="form.icon_type_id == 1">
                                        <label for="exampleInputEmail1">Icono</label>
                                        <input ref="file" accept="image/png" type="file" class="form-control" v-on:change="onFileChange">
                                    </div>
                                    <div class="col-sm-6" v-if="form.icon_type_id == 2">
                                        <label for="exampleInputEmail1">Fa Icon - <a href="https://fontawesome.com/icons" target= "_blank">Ver iconos</a></label>
                                        <input
                                            type="text" 
                                            v-model="form.fai" 
                                            class="form-control"
                                            placeholder="Ingresa el icono"
                                        >
                                    </div>
                                    <div class="col-sm-6" v-if="form.icon_type_id == 3">
                                        <label for="exampleInputEmail1">Ionic Icon - <a href="https://ionicframework.com/docs/v3/ionicons/" target= "_blank">Ver iconos</a></label>
                                        <input
                                            type="text" 
                                            v-model="form.fai" 
                                            class="form-control"
                                            placeholder="Ingresa el icono"
                                        >
                                    </div>
                                </div>
                                <div class="form-group row">
                                    <div class="col-sm-6">
                                        <label for="exampleInputEmail1">¿Es directo a Contenido? <h6 class="m-0 text-danger float-right">*</h6></label>
                                        <select class="form-control" id="exampleFormControlSelect1"
                                        v-model="form.direct_content_question_id"
                                        >
                                            <option :value="1">Si</option>
                                            <option :value="2">No</option>
                                        </select>
                                    </div>
                                </div>
                                <div class="form-group row">
                                    <div class="col-sm-6">
                                        <label for="exampleInputEmail1">¿Es un enlace a una página externa? <h6 class="m-0 text-danger float-right">*</h6></label>
                                        <select class="form-control" id="exampleFormControlSelect1"
                                        v-model="form.link_question_id"
                                        >
                                            <option :value="1">Si</option>
                                            <option :value="2">No</option>
                                        </select>
                                    </div>
                                    <div class="col-sm-6" v-if="form.link_question_id == 1">
                                        <label for="exampleInputEmail1">Url o enlace (Ejemplo: https://google.com)</label>
                                        <input
                                            type="text" 
                                            v-model="form.url"
                                            class="form-control"
                                            placeholder="Ingresa la url o enlace"
                                        >
                                    </div>
                                </div>
                                <div class="form-group row">
                                    <div class="col-sm-6">
                                        <label for="exampleInputEmail1">¿Es un Iframe? <h6 class="m-0 text-danger float-right">*</h6></label>
                                        <select class="form-control" id="exampleFormControlSelect1"
                                        v-model="form.iframe_question_id"
                                        >
                                            <option :value="1">Si</option>
                                            <option :value="2">No</option>
                                        </select>
                                    </div>
                                    <div class="col-sm-6" v-if="form.iframe_question_id == 1">
                                        <label for="exampleInputEmail1">Url del Iframe</label>
                                        <input
                                            type="text" 
                                            v-model="form.iframe"
                                            class="form-control"
                                            placeholder="Ingresa la url o enlace"
                                        >
                                    </div>
                                </div>
                                <div class="form-group row">
                                    <div class="col-sm-6">
                                        <label for="exampleInputEmail1">¿Es un video de Youtube? <h6 class="m-0 text-danger float-right">*</h6></label>
                                        <select class="form-control" id="exampleFormControlSelect1"
                                        v-model="form.youtube_question_id"
                                        >
                                            <option :value="1">Si</option>
                                            <option :value="2">No</option>
                                        </select>
                                    </div>
                                    <div class="col-sm-6" v-if="form.youtube_question_id == 1">
                                        <label for="exampleInputEmail1">URL del Video</label>
                                        <input
                                            type="text" 
                                            v-model="form.video_id"
                                            class="form-control"
                                            placeholder="Ingresa el Id del Video"
                                        >
                                    </div>
                                </div>
                                <button 
                                type="submit"
                                class="btn btn-success btn-icon-split">
                                    <span class="icon text-white-50">
                                        <i class="fas fa-check"></i>
                                    </span>
                                    <span class="text">Guardar</span>
                                </button>
                                <router-link to="/section" class="btn btn-danger btn-icon-split">
                                    <span class="icon text-white-50">
                                        <i class="fas fa-times"></i>
                                    </span>
                                    <span class="text">Cancelar</span>
                                </router-link>
                            </form>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    
</template>

<script>
    import { ClipLoader } from 'vue-spinner/dist/vue-spinner.min.js';
    import InputColorPicker from 'vue-native-color-picker';

    export default {
        components: {
            ClipLoader,
            "v-input-colorpicker": InputColorPicker
        },
        created() {
            this.storeAudit();
        },
        data: function() {
            return {
                errors: [],
                color: '#0A2787',
                loading: false,
                color: "#0f4c81",
                noFile: false,
                form: {
                    title: '',
                    color: '',
                    icon: '',
                    position: '',
                    icon_type_id: null,
                    fai: '',
                    link_question_id: 2,
                    video_id: '',
                    youtube_question_id: 2,
                    google_tag: '',
                    subtitle: '',
                    iframe_question_id: 2,
                    iframe: '',
                    direct_content_question_id: 2
                }
            }
        },
        methods: {
            storeAudit() {
                let formData = new FormData();
                formData.append('page', 'CreateSection');
               
                axios.post('/api/audit/store?api_token='+App.apiToken, formData)
                .then(function (response) {
                    currentObj.success = response.data.success;
                })
                .catch(function (error) {
                    console.log(error);
                });
            },
            handleChange() {
                this.form.color = this.color;
            },
            onFileChange(e){
                this.file = e.target.files[0];
                this.noFile = e.target.files.length;
            },
            onSubmit(e) {
                this.loading = true; //the loading begin
                e.preventDefault();
                let currentObj = this;
    
                const config = {
                    headers: { 'content-type': 'multipart/form-data' }
                }

                if(this.form.title != ''
                    && this.form.color != ''
                    && this.form.google_tag != ''
                    && this.form.icon_type_id != null
                    && (this.file != null || this.form.fai != '')
                    && this.form.position != ''
                ) {
                    let formData = new FormData();
                    formData.append('title', this.form.title);
                    formData.append('color', this.form.color);
                    formData.append('icon_type_id', this.form.icon_type_id);
                    if(this.form.icon_type_id == 1) {
                        formData.append('file', this.file);
                    } else {
                        formData.append('icon', this.form.fai);
                    }
                    formData.append('position', this.form.position);
                    formData.append('link_question_id', this.form.link_question_id);
                    formData.append('url', this.form.url);
                    formData.append('video_id', this.form.video_id);
                    formData.append('subtitle', this.form.subtitle);
                    formData.append('iframe', this.form.iframe);
                    formData.append('google_tag', this.form.google_tag);
                    formData.append('direct_content_question_id', this.form.direct_content_question_id);

                    axios.post('/api/section/store?api_token='+App.apiToken, formData, config)
                    .then(function (response) {
                        currentObj.success = response.data.success;
                    })
                    .catch(function (error) {
                        console.log(error);
                    })
                    .finally(() => {
                        this.loading = false;
                        this.$awn.success("El registro ha sido agregado", {labels: {success: "Éxito"}});
                        this.$router.push('/section');
                    });
                } else {
                    this.loading = false;
                    this.errors = [];
                    
                    if (this.form.title == '') {
                        this.errors.push('El título es obligatorio.');
                    }
                    if (this.form.google_tag == '') {
                        this.errors.push('La etiqueta de Google es obligatoria.');
                    }
                    if (this.form.title.length > 36) {
                        this.errors.push('El nombre debe tener menos de 36 caracteres.');
                    }
                    if (this.form.color == '') {
                        this.errors.push('El color es obligatorio.');
                    }
                    if (this.form.icon_type_id == null) {
                        this.errors.push('El tipo de icono es obligatorio.');
                    }
                    if (this.form.icon_type_id == 1 && this.file == null) {
                        this.errors.push('El icono es obligatorio.');
                    }
                    if (this.form.icon_type_id == 2 && this.form.fai == '') {
                        this.errors.push('El icono es obligatorio.');
                    } 
                    if (this.form.icon_type_id == 1 && (this.file.size > 1024 * 1024)) {
                        this.errors.push('La imagen es muy pesada.');
                    }
                    if (this.form.position == '') {
                        this.errors.push('La posición es obligatoria.');
                    }

                    $('html,body').scrollTop(0);

                    e.preventDefault();
                }
            },

        },
        computed: {
            isDisabled() {
                return true;
            },
            charactersLeft() {
                var char = this.form.title.length,
                    limit = 36;

                return (limit - char) + " / " + limit + " caracteres disponibles";
            }
        }
    }
</script>
<style lang="scss">
    @import '~vue-awesome-notifications/dist/styles/style.scss';
</style>
