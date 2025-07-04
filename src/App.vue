<script setup lang="ts">
import { ref, onMounted } from "vue";
import BpmnModeler from "bpmn-js/lib/Modeler";
// @ts-ignore
import camundaModdleDescriptor from "camunda-bpmn-moddle/resources/camunda";
import xml from "./diagram";

const continon = ref();
const bpmnModeler = ref();
const workflowButtonType = [
  {
    name: "ButtonList",
    supperClass: ["Element"],
    meta: {
      allowedIn: ["*"],
    },
    properties: [
      {
        name: "values",
        type: "Button",
        isMany: true,
      },
    ],
  },
  {
    name: "Button",
    supperClass: ["Element"],
    meta: {
      allowedIn: ["camunda:ButtonList"],
    },
    properties: [
      {
        name: "buttonStyles",
        type: "ButtonStyle",
        isMany: true,
      },
      {
        name: "buttonFunctionRelateLists",
        type: "ButtonFunctionRelateList",
        isMany: true,
      },
      {
        name: "identity",
        type: "String",
        isAttr: true,
      },
      {
        name: "name",
        type: "String",
        isAttr: true,
      },
      {
        name: "ownerId",
        type: "String",
        isAttr: true,
      },
    ],
  },
  {
    name: "ButtonStyle",
    supperClass: ["Element"],
    meta: {
      allowedIn: ["camunda:Button"],
    },
    properties: [
      {
        name: "color",
        type: "String",
        isAttr: true,
      },
      {
        name: "type",
        type: "String",
        isAttr: true,
      },
      {
        name: "plain",
        type: "String",
        isAttr: true,
      },
    ],
  },
  {
    name: "ButtonFunctionRelateList",
    supperClass: ["Element"],
    meta: {
      allowedIn: ["camunda:Button"],
    },
    properties: [
      {
        name: "workflowButtonFunctionIdentity",
        type: "String",
        isAttr: true,
      },
    ],
  },
];
onMounted(() => {
  const customCamundaModdleDescriptor = JSON.parse(
    JSON.stringify(camundaModdleDescriptor)
  );
  customCamundaModdleDescriptor.types.push(...workflowButtonType);
  bpmnModeler.value = new BpmnModeler({
    container: continon.value,
    additionalModules: [],
    moddleExtensions: {
      camunda: customCamundaModdleDescriptor,
    },
  });
  bpmnModeler.value.importXML(xml).then((res: any) => {
    // @ts-ignore
    console.log(res, "res");
  });
});
const getXml = async () => {
  const { xml } = await bpmnModeler.value.saveXML({ format: true });
  // @ts-ignore
  console.log(xml, "getXml");
  return xml;
};
</script>

<template>
  <div ref="continon" @click="getXml" class="box"></div>
</template>

<style scoped>
.box {
  height: 100vh;
  width: 100vw;
}
</style>
