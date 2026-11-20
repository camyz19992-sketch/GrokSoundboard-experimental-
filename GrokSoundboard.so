using System.Collections;
using System.Collections.Generic;
using System.IO;
using UnityEngine;
using UnityEngine.Networking;
using UnityEngine.UI;
using UnityEngine.XR;
using TMPro;

public class GrokSoundboard : MonoBehaviour
{
    [Header("Menu Settings")]
    public KeyCode editorToggleKey = KeyCode.Y;
    public XRNode controllerHand = XRNode.LeftHand;
    public InputHelpers.Button toggleButton = InputHelpers.Button.PrimaryButton; // Y

    [Header("UI References")]
    public GameObject menuRoot;
    public TextMeshProUGUI titleText;
    public Transform buttonContainer;
    public GameObject soundButtonPrefab;   // simple button prefab
    public Image backgroundImage;
    public float strobeSpeed = 2.5f;

    [Header("Sound Loading")]
    public bool loadFromPersistentData = true;
    public string folderName = "Soundboard";

    private List<AudioClip> loadedClips = new List<AudioClip>();
    private List<string> clipNames = new List<string>();
    private AudioSource source;
    private bool menuOpen;
    private bool wasPressed;
    private string soundFolder;
    private float hue;

    void Awake()
    {
        source = gameObject.AddComponent<AudioSource>();
        source.playOnAwake = false;
        source.spatialBlend = 0f;

        soundFolder = Path.Combine(Application.persistentDataPath, folderName);
        if (!Directory.Exists(soundFolder))
            Directory.CreateDirectory(soundFolder);

        if (menuRoot != null)
            menuRoot.SetActive(false);

        if (titleText != null)
            titleText.text = "Grok Soundboard";

        if (loadFromPersistentData)
            StartCoroutine(LoadAllSounds());
    }

    void Update()
    {
        // Toggle menu
        bool pressed = Input.GetKeyDown(editorToggleKey);

        if (!pressed && toggleButton != InputHelpers.Button.None)
        {
            InputHelpers.IsPressed(InputDevices.GetDeviceAtXRNode(controllerHand), toggleButton, out pressed);
            if (pressed && !wasPressed) ToggleMenu();
            wasPressed = pressed;
        }
        else if (pressed)
        {
            ToggleMenu();
        }

        // RGB Strobe
        if (menuOpen && backgroundImage != null)
        {
            hue += Time.deltaTime * strobeSpeed;
            if (hue > 1f) hue -= 1f;
            backgroundImage.color = Color.HSVToRGB(hue, 0.85f, 0.35f);
        }
    }

    void ToggleMenu()
    {
        menuOpen = !menuOpen;
        if (menuRoot != null)
            menuRoot.SetActive(menuOpen);
    }

    IEnumerator LoadAllSounds()
    {
        loadedClips.Clear();
        clipNames.Clear();

        // Clear old buttons
        if (buttonContainer != null)
        {
            foreach (Transform child in buttonContainer)
                Destroy(child.gameObject);
        }

        string[] files = Directory.GetFiles(soundFolder);
        foreach (string file in files)
        {
            string ext = Path.GetExtension(file).ToLower();
            if (ext != ".wav" && ext != ".ogg") continue;

            string url = "file://" + file;
            using (UnityWebRequest www = UnityWebRequestMultimedia.GetAudioClip(url, AudioType.UNKNOWN))
            {
                yield return www.SendWebRequest();

                if (www.result == UnityWebRequest.Result.Success)
                {
                    AudioClip clip = DownloadHandlerAudioClip.GetContent(www);
                    string name = Path.GetFileNameWithoutExtension(file);

                    loadedClips.Add(clip);
                    clipNames.Add(name);

                    CreateSoundButton(name, loadedClips.Count - 1);
                    Debug.Log("Loaded: " + name);
                }
            }
        }

        Debug.Log($"Grok Soundboard loaded {loadedClips.Count} sounds");
    }

    void CreateSoundButton(string label, int index)
    {
        if (soundButtonPrefab == null || buttonContainer == null) return;

        GameObject btnObj = Instantiate(soundButtonPrefab, buttonContainer);
        btnObj.SetActive(true);

        TextMeshProUGUI txt = btnObj.GetComponentInChildren<TextMeshProUGUI>();
        if (txt != null) txt.text = label;

        Button btn = btnObj.GetComponent<Button>();
        if (btn != null)
        {
            int capture = index;
            btn.onClick.AddListener(() => Play(capture));
        }
    }

    public void Play(int index)
    {
        if (index < 0 || index >= loadedClips.Count || loadedClips[index] == null) return;
        source.PlayOneShot(loadedClips[index]);
    }

    public void ReloadSounds()
    {
        StartCoroutine(LoadAllSounds());
    }
}